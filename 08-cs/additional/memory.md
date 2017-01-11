## Memory

### The Stack and Heap

* Out of Memory
* Stack overflow
* Heap overflow

The above are messages that you may encounter or have already encountered when working with programs. So what do they mean?

Whenever a program initialized (let's say, a Ruby program), the computer allocates a block of memory for the program to run in.

![Memory](http://www.cs.cornell.edu/courses/cs312/2004fa/lectures/memory%20layout.jpg)

The block of memory is separated into 3 sections: the stack, the heap, and the code.
The code required to run the program, and the stack/heap are for allocating new values. Note that the stack grows in one direction, while the heap grows in the other direction.
If one of these encroaches on the other, we run out of memory!

Ideally, this shouldn't happen, so we need to make sure that we only allocate memory when we absolutely need it. Web programmers occasionally encounter these problems, so it's good
to have a general idea of what's going on under the hood.

Also note that linked lists, graphs, trees, and any other data structure that relies on **pointers** (variables that aren't values, but memory addresses) will usually store data in the heap.

### Memory Hierarchy

![Memory Hierarchy](http://tjliu.myweb.hinet.net/COA_CH_6.files/image007.jpg)
