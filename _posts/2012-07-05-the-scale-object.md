---
layout: post
title: "The scale~ object"
date: 2012-07-05
categories: 74objects
source: "Originally published on 74objects.com"
---

[Max 6](http://cycling74.com/products/max/) introduced some big changes when it was released last fall.  Flying under-the-radar were a number of new objects added to the collection.  A few of these objects came from [Tap.Tools](http://shop.electrotap.com/products/taptools), including the **join**/**unjoin** objects and the **XmlParse** class for MXJ.  In the MSP world, the **scale~** object entered the fray.

On the surface, **scale~** is relatively straightforward.  It maps one range onto another.  With the addition of the exponent to provide non-linear functions the object becomes much more powerful, and little more difficult to quantify as a mathematical formula.  More difficult still is a backwards-compatibility mode called “classic” which implements an idiosyncratic exponential base function from back in the ancient days of the [ISPW](http://en.wikipedia.org/wiki/ISPW).  You probably don’t want to use this mode, but I’ve included it here for thoroughness sake.

So let’s take a technical look at **scale~** beginning with the equation it implements.

[![scale~ equations](https://web.archive.org/web/2013if_//assets/images/74objects/2012-07-Screen-Shot-2012-07-05-at-10.14.33-AM.png)](/assets/images/74objects/2012-07-Screen-Shot-2012-07-05-at-10.14.33-AM.png)This rendering was typeset in TextMate using the MacTex distribution of Latex.  Here is the Latex source:

```
\section{modern scale$\sim$} %(fold)

\begin{equation}
y = out_{low} +  
 \left( out_{high} - out_{low} \right)  \left( \frac{ x - in_{low} }{ in_{high} - in_{low} } \right) ^{exp}
 \text{,} \qquad \frac{ x - in_{low} }{ in_{high} - in_{low} } > 0
\end{equation}

\begin{equation}
y = out_{low} + \left( out_{high} - out_{low} \right) \left( -1 \right)  
 \left( \frac{ -x + in_{low} }{ in_{high} - in_{low} } \right) ^{exp} \text{,}
 \qquad \frac{ x - in_{low} }{ in_{high} - in_{low} } < 0 \\
\end{equation}

\section{classic scale$\sim$} %(fold)

\begin{equation}
y = out_{low} + \left( out_{high} - out_{low} \right)  
 \bigg[ (out_{high} - out_{low})  \exp(-(in_{high}-in_{low}) \log(exp))  \exp(x \log(exp)) \bigg]  
 \text{,} \qquad  out_{high} - out_{low} \geq 0
\end{equation}

\begin{equation}
y = -\bigg(out_{low} + \left( out_{high} - out_{low} \right)  
 \bigg[ (out_{high} - out_{low})  \exp(-(in_{high}-in_{low}) \log(exp))  \exp(x \log(exp)) \bigg]  
 \bigg) \text{,} \qquad  out_{high} - out_{low} < 0
\end{equation}
```

Finally, we have something functional to use: code for use in [Octave](http://www.gnu.org/software/octave/) or Matlab.

```
function [ y ] = scale_modern( x, in_low, in_high, out_low, out_high, exp )
 if (((x-in_low)/(in_high-in_low)) > 0)
 y = out_low + (out_high-out_low) * ((x-in_low)/(in_high-in_low))^exp;
 elseif (((x-in_low)/(in_high-in_low)) < 0)
 y = out_low + (out_high-out_low) * -((((-x+in_low)/(in_high-in_low)))^(exp));
 else
 y = out_low;
 end
end

function [ y ] = scale_classic( x, in_low, in_high, out_low, out_high, power )
 if (out_high-out_low >= 0)
 y = out_low + (out_high-out_low) * (  (out_high - out_low) *
 exp(-1*(in_high-in_low)*log(power)) * exp(x*log(power)) );
 else
 y = (-1) * ( out_low + (out_high-out_low) * (  (out_high - out_low) *
 exp(-1*(in_high-in_low)*log(power)) * exp(x*log(power)) ) );
 end
end
```

Scaling signals isn’t particularly sexy, but it’s certainly handy item to have in the tool belt.  One more of those little sneaky features in Max 6 that makes it an awesome upgrade.
