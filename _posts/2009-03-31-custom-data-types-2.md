---
layout: post
title: "Custom Data-Types in Max Part 2: Nobox Classes"
date: 2009-03-31
categories: 74objects
source: "Originally published on 74objects.com"
---

In this series I am offering a few answers to the question “what’s the best way to have multiple max objects refering to a custom data structure?”  Another variation on that question is ”I want to define a class that will *never* become a full-blown external for instantiation in a Max patcher, but will be instantiated invisibly as a (possibly singleton) object that can serve some functions for other objects.”  In essence, the answer to both of these questions begins with the creation of ‘nobox’ objects.

This is the second of a multi-part series. Over the next several weeks I will be writing about several different approaches to passing custom data types in Max, and I’ll be using some real-world examples to demonstrate how and why these various strategies are effective.

- [Introduction](//web.archive.org/web/20131215234158/http://blog.74objects.com/2009/03/25/custom-data-types-1/)
- [Creating “nobox” classes](//web.archive.org/web/20131215234158/http://blog.74objects.com/2009/03/31/custom-data-types-2/)
- [Binding to symbols](//web.archive.org/web/20131215234158/http://blog.74objects.com/2009/04/07/custom-data-types-3/) (e.g. table, buffer~, coll, etc.)
- [Passing objects directly](/web/20131215234158/http://74objects.com/2010/02/06/custom-data-types-4) (e.g. Jamoma Multicore)
- Hash-based reference system (similar to Jitter)

## Boxless Objects

When people use Max they typically think about objects, created in a little ‘boxes’ in a patcher document.  These boxes are then connected with patch cords.

In the [first part of this series](/74objects/2009/03/25/custom-data-types-1.html) I introduced a new data type called a ‘frog’.

The the frog class could be defined as a C++ object, a C struct, or some other way. We will define our custom ‘frog’ type as a Max class.  In Max there are two common ways to define a class.  The first is a “box” class, which is to say that it is an object that can be instantiated in a box that is in a patcher.  Most objects are box classes.

The second way is to create a “nobox” class.  A nobox is a class that cannot be created directly in a Max patcher. Instead this is a class that exists solely under-the-hood to be used by other classes, or by Max itself. We will create our ‘frog’ data type as a ‘nobox’ class.

One example of a nobox class that is defined internally to Max is the t_atomarray in the Max API.  Let’s consider it’s definition from ext_atomarray.h:

```
typedef struct _atomarray {
	t_object	ob;
	long		ac;
	t_atom		*av;
} t_atomarray;
```

The atomarray is simply an object that manages an array of atoms.  However it is a proper object. You can instantiate it by calling object_new() and you free it by calling object_free().  It has a the typical Max methods which can be invoked by sending messages to the object using the object_method(), object_method_typed(), and other similar functions.

If you poke around the Max SDK you will probably notice a number of these nobox classes.  They include t_linklist, t_hashtab, t_dictionary, t_symobject, etc.  Even *The* Max object, the one that you send cryptic messages to using message boxes that say things like “;max sortpatcherdictonsave 1″, is a nobox object.

## Defining a Frog

If we define our frog as a nobox class, we may have a struct like this:

```
typedef struct _frog {
	t_object	ob;
	long		num_flies;
 	t_atom		*flies;
} t_frog;
```

This is basically the same thing as an atomarray, but we will make it ourselves from scratch.  And we can define some more whimsical names for its methods.

Just like any other class, we need to cache our class definition in a static or global toward the top of our file.  So we can simply do that like usual:

```
t_class *s_frog_class = NULL;
```

Then we can get to the class definition, which once again will look pretty familiar to anyone who has written a few objects using the Max 5 SDK.

```
int main(void)
{
	common_symbols_init();

	s_frog_class = class_new("frog",
				(method)frog_new,
				(method)frog_free,
				sizeof(t_frog),
				(method)NULL,
				A_GIMME,
				0L);

	class_addmethod(s_frog_class, (method)frog_getflies, 	"getflies", A_CANT, 0);
	class_addmethod(s_frog_class, (method)frog_appendfly, 	"appendfly", A_CANT, 0);
	class_addmethod(s_frog_class, (method)frog_getnumflies,	"getnumflies", 0);
	class_addmethod(s_frog_class, (method)frog_clear,	"clear", 0);

	class_register(_sym_nobox, s_frog_class);
	return 0;
}
```

Essentially:

- we define the class itself, which includes providing the instance create and destroy methods, the size of the object’s data, and what kind of arguments the creation method expects.
- we initialize commonsyms — this means we can refer to a whole slew of pre-defined symbols without having to make computationally expensive gensym() calls.  For example, we can use _sym_nobox instead of gensym(“nobox”).
- we add some message bindings so that we can call the methods using object_method() and friends.  One aspect of these messages is that we gave a couple of them A_CANT types.  This is uncommon for normal box classes, but quite common for nobox classes.  It essentially indicates that Max “can’t” typecheck the arguments.  This allows us to bind the message to a method with virtually any prototype we want.
- **we register the class as a *****nobox***** object**

Take special note of that last step.  Instead of registering the class in the “box” namespace, we register it in the “nobox” namespace.

We could also define attributes for our class, but for the sake of simplicity we are just using messages in this example.

## The Froggy Lifecycle

When we go to use our frog class we will expect to be able to do the following:

```
t_object *myfroggy;
myfroggy = object_new_typed(_sym_nobox, gensym("frog"), 0, NULL);

// do a bunch of stuff
// snap up some flies
// sit around the pond and talk about how the mud was in the good ole days...

object_free(myfroggy);
```

Notice that once again we have to specify the correct namespace for the object, _sym_nobox, in our call to object_new_typed().  We used object_new_typed() because we defined the class to take arguments in the A_GIMME form.  If we use object_new() instead of object_new_typed() then the arguments passed to our instance creation routine would be pointing to bogus memory (and we *definitely* do not want that – unless you are a crash loving masochist).

Speaking of the object creation routine, it can be pretty simple:

```
t_object* frog_new(t_symbol *name, long argc, t_atom *argv)
{
 t_frog	*x;

 x = (t_frog*)object_alloc(s_frog_class);
 if(x){
		// in Max 5 our whole struct is zeroed by object_alloc()
		// ... so we don't need to do that manually

		// handle attribute arguments.
		// we don't have any attributes now, but we might add some later...
		attr_args_process(x, argc, argv);
	}
	return x;
}
```

In addition to the things noted in the method’s code, I’ll point out the obvious fact that we don’t need to worry about creating inlets or outlets — our object will never be visible in a box in a patcher, and thus never have patch cords connected to it.

Our free method is also quite simple.  We just call the clear method.

```
void frog_free(t_frog *x)
{
	frog_clear(x);
}
```

## Sending Messages to a Frog

At the beginning of the previous section we created an instance of the frog object with object_new_typed().  We probably didn’t do this just to free the object again.  We want to send some messages to get our frog to do something – like collect flies.

Let’s define the four methods we specified above:

```
void frog_getflies(t_frog *x, long *numflies, t_atom **flies)
{
	if (numflies && flies) {
		numflies = x->num_flies;
		flies = x->flies;
	}
}

void frog_appendfly(t_frog *x, t_atom *newfly)
{
	if (x->num_flies == 0) {
		x->num_flies = 1;
		flies = (t_atom*)sysmem_newptr(x->num_flies * sizeof(t_atom));
	}
	else {
		x->num_flies++;
		flies = (t_atom*)sysmem_resizeptr(flies, x->num_flies * sizeof(t_atom));
	}
	x->flies[x->num_flies-1].a_type = newfly->a_type;
	sysmem_copyptr(newfly, x->flies+(x->num_flies-1), sizeof(t_atom));
}

long frog_getnumflies(t_frog *x)
{
	return x->num_flies;
}

void frog_clear(t_frog *x)
{
	if (x->num_flies && x->flies) {
		sysmem_freeptr(x->flies);
		x->flies = NULL;
		x->num_flies = 0;
	}
}
```

None of the messages are defined with argument types of A_GIMME, so we will use object_method() to send messages, and not *object_method_typed()* or its immediate descendants.  Usage of these methods might then look like this:

```
t_atom	mosquito;
t_atom	bee;
long	n;
long	ac = 0;
t_atom	*av = NULL;

atom_setsym(&mosquito, gensym("eeeeyeyeeyyeyyyyeeyyye"));
atom_setsym(&bee, gensym("bzzzzzzzz"));
// here we have the frog snap up the flies using one its A_CANT methods
object_method(myfroggy, gensym("appendfly"), &mosquito);
object_method(myfroggy, gensym("appendfly"), &bee);

// this call returns a value -- we have to cast it, but that's okay
n = (long)object_method(myfroggy, gensym("getnumflies"));
// another A_CANT method, passing two pointer args
object_method(myfroggy, gensym("getflies"), &ac, &av);

// we're all done and the froggy has a bowel movement
object_method(myfroggy, gensym("clear"));
```

## Wrap Up

So we have a boxless class now.  It isn’t all that different from a regular class, but there are always people asking me about example code that shows how to do this sort of thing.  And this information lays the foundation for the upcoming articles in this series.

If you have any questions, please leave a comment!
