---
layout: post
title: "Custom Data-Types in Max Part 3: Binding to Symbols"
date: 2009-04-07
categories: 74objects
source: "Originally published on 74objects.com"
---

When people design systems in Max that are composed of multiple objects that share data, they have a problem: how do you share the data between objects? The coll object, for example, can share its data among multiple coll objects with the same name.  The buffer~ object can also do this, and other objects like play~ and poke~ can also access this data.  These objects share their data by binding themselves to symbols.

This is the third article in a series about working with custom data types in Max.  In the first two articles we laid the groundwork for the various methods by discussing how we wrap the data that we want to pass.  The next several articles will be focusing on actually passing the custom data between objects in various ways.  In this series:

- [Introduction](//web.archive.org/web/20131216003937/http://blog.74objects.com/2009/03/25/custom-data-types-1/)
- [Creating “nobox” classes](//web.archive.org/web/20131216003937/http://blog.74objects.com/2009/03/31/custom-data-types-2/)
- [Binding to symbols](//web.archive.org/web/20131216003937/http://blog.74objects.com/2009/04/07/custom-data-types-3/) (e.g. table, buffer~, coll, etc.)
- [Passing objects directly](/web/20131216003937/http://74objects.com/2010/02/06/custom-data-types-4) (e.g. Jamoma Multicore)
- Hash-based reference system (similar to Jitter)

## The Symbol Table

Before we can talk about binding objects to symbols, we should review what a symbol is and how Max’s symbol table works. First, let’s consider the definition of a t_symbol from the Max API:

```
typedef struct _symbol {
 char *s_name;
 t_object *s_thing;
} t_symbol;
```

So the t_symbol has two members: a pointer to a standard C-string and a pointer to a Max object instance.  We never actually create, free, or otherwise manage t_symbols directly.  This is a function of the Max kernel.  What we do instead is call the gensym() function and Max will give us a pointer to a t_symbol that is resident in memory, like so:

```
t_symbol *s;

s = gensym("ribbit");
```

What is it that gensym() does exactly?  I’m glad you asked…  The gensym() function looks in a table maintained by Max to see if this symbol exists in that table.  If it does already exist, then it returns a pointer to that t_symbol.  If does not already exist then it creates it, adds it to the table, and then returns the pointer.  In essence, it is a [hash table](http://en.wikipedia.org/wiki/Hash_table) that maps C-strings to t_symbol struct instances.

*As a side note, one of the really fantastic improvements introduced in Max 5 is dramatically faster performance from the gensym() function and the symbol table.  It’s not a sexy feature you will see on marketing materials, but it is one of the significant under-the-hood features that make Max 5 such a big upgrade.*

As seasoned developers with the Max or Pd APIs will know, this makes it extremely fast to compare textual tidbits.  If you simply try to match strings with strcmp(), then each character of the two strings you are comparing will need to be evaluated to see if they match.  This is not a fast process, and Max is trying to do things in real time with a huge number of textual messages being passed between objects.  Using the symbol table, you can simply compare two t_symbol pointers for equality.  One equality check and you are done.

The symbol table is persistent throughout Max’s life cycle, so every symbol gensym()’d into existance will be present in the symbol table until Max is quit.  This has the benefit of knowing that you can cache t_symbol pointers for future comparisons without worrying about a pointer’s future validity.

## There’s s_thing You Need to Know

So we have seen that Max maintains a table of t_symbols, and that we can get pointers to t_symbols in the table by calling the gensym() function.  Furthermore, we have seen that this is a handy and fast way to deal with strings that we will be re-using and comparing frequently.  That string is the s_name member of the t_symbol.  Great!

Now lets think about the problem we are trying to solve.  In the first part of this series we established that we want to have a custom data structure, which we called a ‘frog’.  In the second part of this series we implemented that custom data structure as a boxless class, which is to say it is Max object.  An now we need a way to access our object and share it between other objects.

You are probably looking at the s_thing member of the t_symbol and thinking, “I’ve got it!”  Well, maybe.  Let’s imagine that we simply charge ahead and start manipulating the s_thing member of our t_symbol.  If we did our code might look like this:

```
t_symbol *s;
t_object *o;

s = gensym("ribbit");
o = object_new_typed(_sym_nobox, gensym("frog"), 0, NULL);
s->s_thing = 0;
```

Now, in some other code in some other object, anywhere in Max, you could have code that looks like this:

```
t_symbol *s;
t_object *o;

s = gensym("ribbit");
o = s->s_thing;

// o is now a pointer to an instance of our frog
// which is created in another object
```

Looks good, right? That’s what we want. Except that we’ve made a lot of assumptions:

- We aren’t checking the s->s_thing before we assign it our frog object instance.  What if it already has a value?  Remember that the symbol table is global to all of Max.  If there is a buffer~, or a coll, or a table, or a detonate object, (etc.) bound to the name “ribbit” then we just broke something.
- In the second example, where we assign the s_thing to the o variable, we don’t check that the s_thing actually is anything.  It could be NULL.  It could be an object other than the frog object that we think it is.
- What happens if we assign the pointer to o in the second example and then the object is freed immediately afterwards in another thread before we actually start dereferencing our frog’s member data?  This thread-safety issue is not academic – events might be triggered by the scheduler in another thread or by the audio thread.

So clearly we need to do more.

## Doing More

Some basic sanity checks are in order, so let’s see what the Max API has to offer us:

- First, we should check if the s_thing is NULL.  Most symbols in Max will probably have a NULL s_thing, because most symbols won’t have objects bound to them.
- If it is something, there is no guarantee that the pointer is pointing to a valid object.  You can use the NOGOOD macro defined in ext_mess.h to find out.  If you pass a pointer to NOGOOD then it will return true if the pointer is, um, no good.  Otherwise it returns false – in which case you are safe.
- If you want to be safe in the event that you have multiple objects accessing your object, then you may want to incoporate some sort of reference counting or locking of your object.  This will most likely involve adding a member to your struct which is zero when nothing is accessing your object (in our case the frog), and non-zero when something is accessing it.  You can use ATOMIC_INCREMENT and ATOMIC_DECREMENT (defined in ext_atomic.h) to modify that member in a thread safe manner.
- Finally, there is the “globalsymbol” approach is demonstrated in an [article about safely accessing buffer~ data](http://blog.74objects.com/2009/03/22/accessing-buffers) that appeared here a few weeks ago.

## Alternative Approaches

There are some alternative approaches that don’t involve binding to symbols.  For example, you could have a class that instead implements a hash table that maps symbols to objects.  This would allow you to have named objects in a system that does not need to concern itself with conflicts in the global namespace.  This is common practice in the [Jamoma Modular](http://jamoma.org/) codebase.

The next article in this series will look at a very different approach where the custom data is passed through a chain of objects using inlets and outlets. Stay tuned…
