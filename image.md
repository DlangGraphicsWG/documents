# What is an Image?

- View!Color (and operations on these) are range-like objects that can be composed like in `ae.utils.graphics`

  * Pros:
      - Friendly and efficient UFCS chains of operations (need good names).
      - The Pros of Input ranges, but for images.

  * Cons:
      - UFCS chains may leave no room to pass a complex context.
      - Most of algorithms end up being written for contiguous in-memory images anyway.
      - Like `std.range`, functions are all defined externally hence not quite easy to discover.

- Images types should have a runtime interface. This interface is a POD `struct`
  that looks like:
```d
    struct RuntimeImage
    {
        int width, height;
        size_t pitchbetweenRows;
        void* data;
        PixelFormat whateverIsNeeded;
    }
```

  RuntimeImage will get the easiest name, not View!T.

  * Pros:
      - Simplest thing that works.
      - You can avoid to template something by the Color type.
      - Allows crossing binary interfaces, like shared libraries and C ABI.
      - Format conversion may need this internally.
      - This "type-punning" reduce template bloat and compile times.
      - not an `interface` so avoid classes

  * Cons:
      - More work.
      - The runtime interface l virtual dispatch.
      - not an `interface` so inheritance is limited

- a buffer to render widgets to, is an Image

- Images should support fast subsets (like slicing an array)

- an Image should have named color channels for easy indexing

- Image data should be held by an `IAllocator`


# Open questions (no vote occured yet)

- Who owns `RuntimeImage` data?

- whether the Image abstraction is 2D (N = 2), 3D-or-less (N <= 3) or N-dimensional (any N)

- what exactly owns IAllocator?

- who owns the Image abstraction itself (GC, RC, scoped ownership)?

- how animated Image would be represented?
