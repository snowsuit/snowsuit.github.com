---
layout: post
title: OCaml Ancient
categories: posts
author: Alex
comments: true
---

OCaml has minor and major heaps. The minor heap is for short-lived things and major is for long-lived things. The [Ancient](http://git.annexia.org/?p=ocaml-ancient.git;a=blob;f=README.txt) library introduces an "ancient" heap for in-memory objects that are too big for memory.

You manually mark objects as ancient. When you do this, the library creates a *new heap that is not traversed by the GC*.

This is implemented as a *library* in OCaml.

That's just crazy.

My friend [Jamie Brandon](https://github.com/jamii/) used it in [texsearch](https://github.com/jamii/texsearch) to stop the GC from trying to traverse a 12 gigabyte suffix tree.

