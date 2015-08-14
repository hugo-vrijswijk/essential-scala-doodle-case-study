# Introduction

This supplement to Essential Scala brings together the book's main concepts in a sizable case study. The object is to develop a *two-dimensional vector graphics* library similar to [Doodle][doodle]. Let us first describe the what two-dimensional graphics are, then why this makes a good case study, and finally describe the structure of the case.

What we mean by two-dimensional graphics should be obvious (though, arguably, when we add animations they become three-dimensional in an unconventional meaning of the term). By vector graphics we mean images that are specified in terms of lines and curves, rather than in terms of pixels. Because of this, vector graphics are independent of the output format. They can be rendered to high resolution screens, such as Apple's Retina displays, as easiily as they can to screens with standard resolution. They can be arbitrarily transformed without losing information, unlike bitmaps that distort when they are rotated, for example. This structure is something our library will heavily leverage.

So, why vector graphics? There are a few reasons. Firstly, graphics are fun, and hopefully something you can enjoy studying even if you haven't worked with computer graphics before. Having a tangible output is of great benefit. It's easier to get a feel for how the library works and concepts it embodies because you simply draw the images on the screen. Finally, we'll see they are great vehicle for the concepts taught in Essential Scala, and we can wrap up most of the content in the course in this one case study.

Now let's give an overview of the structure of the case study. It is divided into three main sections:

1. Building the basic objects and methods for our library, which heavily uses algebraic data types and structural recursion.
2. Adding animations, which introduces sequencing abstractions such as `map` and `fold`.
3. Abstracting the rendering pipeline, bringing in type classes and touching briefly on some more advanced functional programming concepts.

Each part asks you to implement part of the library. Supplementing this supplement is a code repository containing support code as well as working implementations for each exercise. There are many possible implementations for each exercise, each making different design tradeoffs. Our solution is just one of these, and you shouldn't take it to be any more correct than other solutions you may come up with. A primary goal of this supplement is to understand the tradeoffs different solutions make.

To get the most from this case study *you have to do the work*. While it will be valuable if you read through and look at our solutions, it will be much more valuable if you attempt every exercise yourself before looking at our solution.

Finally, we always enjoy talking to other programmers. If you have any comments or questions about this case study, do drop us an email at `noel@underscore.io` and `dave@underscore.io`.