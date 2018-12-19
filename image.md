# What is an Image?

- Images (and Image operations) are range-like objects that can be composed like in `ae.utils.graphics`

  * Pros:
      - Friendly and efficient UFCS chains of operations (need good names).
      - The Pros of Input ranges, but for images.

  * Cons:
      - UFCS chains may leave no room to pass a complex context.
      - Most of algorithms end up being written for contiguous in-memory images anyway.
      - Like `std.range`, functions are all defined externally hence not quite easy to discover.


- Images types should have a runtime interface

  * Pros:
      - The main advantage is that you can avoid to template something by the Image type.
      - (Like std.allocator have IAllocator so that containers can avoid being templated by the allocators specific type)
      - Allows crossing binary interfaces like shared libraries.
      - Format conversion may need this internally.
      - This "type-punning" reduce template bloat and compile times.

  * Cons:
      - More work.
      - The runtime interface has virtual dispatch.

- a buffer to render widgets to, is an Image

- Images should support fast subsets (like slicing an array)

- an Image should have named color channels for easy indexing

- Image data should be held by an `IAllocator`


# Open questions (no vote occured yet)

- whether the Image abstraction is 2D (N = 2), 3D-or-less (N <= 3) or N-dimensional (any N)

- what exactly owns IAllocator?

- who owns the Image abstraction itself (GC, RC, scoped ownership)?

- how animated Image would be represented?
