---
layout: post
title: Three Distributed Systems Papers To Implement
categories: [posts, distributed systems]
author: Malcolm
comments: false
---

[Alex](http://nullspace.io/) sent Avi Bryant's [talk](http://vimeo.com/4763707)
around recently.  One, surprisingly, hard aspect of learning to program is
finding projects to work on.  While there is a lot of enthusiasm, many people
just aren't sure how to spend their time.  Avi's suggestion is to find academic
papers and implement them.

So here are three papers that I think dovetail together and very simple versions
can be implemented in probably a few months.

Dynamo
======

[Paper](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

The Dynamo paper an eventually consistent datastore from Amazon.  There is a lot
going on in this paper, but what it comes down to a key-value store that is
distributed across a set of machines.  Even if the machines cannot talk to each
other, for whatever reason, all machines can accept writes even for the same
keys.  When the machines can talk to each other again, the data is replicated
between them.

A simple way you could implement this is doing everything in-memory.  All data
is replicated across all machines.  If a write comes into one machine it
replicates the write to every other machine.  If a machine joins, they send all
of their data to each other.

Vector Clocks
=============

[Blog Post](http://docs.basho.com/riak/latest/theory/concepts/Vector-Clocks/)

After implementing above, you might notice a problem you: your database is
inconsistent, meaning not all machines are storing the same data.  The key `K`
could map to `value1` on Machine 1 and `value2` on Machine 2.  This could
happen, for example, when two clients write to the same key on different
machines with different values.  When they replicate the value to each other
they overwrite the value they have with the others value.

Vector clocks are a way to solve this.  The Dynamo paper talks about keeping
multiple versions of the data in the case of concurrent writes. Vector Clocks
are a mechanism for representing this versioning.  By using Vector Clocks, you
can track the order of events and specifically when two events happen at the
same time.  This means when someone performs a `GET` operations on a key, they
could receive multiple versions of the data back.  They then have to figure out
how to resolve that conflict and write the correct value back.

CRDTs
=====

[Video](http://research.microsoft.com/apps/video/dl.aspx?id=153540)

[Paper](http://hal.inria.fr/docs/00/60/93/99/PDF/RR-7687.pdf)

Implementing resolvers for data can be annoying, wouldn't it be nice if the
database could do this for you?  CRDTs are 'conflict-free data types' or
'commutative replicative data types' depending on who you ask.  They provide a
way to represent values such that the resolver can be created and applied by the
database.

CRDTs roughly come down to describing datatypes in terms of sets or unordered
lists.  For example, a counter is a list of integers representing the increment
value.  So the value of the counter is the sum of the list.  The paper also
describes how to implement maps and other datatypes.

Conclusion
==========

The above three papers provide a great introduction into how to how a
distributed datastore might work.  The first rule of implementing distributed
systems is generally "be stateless", which isn't possible if you're the
database.  Understanding the problems a distributed database you are using faces
and the solutions is helpful in understanding how to construct your application
logic. For example, if you're using Riak you should understand it's beneficial
to construct your data as a lattice.  If you're using Cassandra, you might lose
writes.  You can learn a lot by spending a few months implementing basic
versions of these tools.
