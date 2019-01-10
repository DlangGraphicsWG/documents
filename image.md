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
      - Allows crossing binary interfaces, like shared libraries and C ABI.
      - Simplest thing that works.
      - You can avoid to template something by the Color type. This "type-punning" reduce template bloat and compile times.     
      - Format conversion may need this internally.
      - Instruction-cache friendly.
      - Not an `interface` so avoid classes and associated woes.

  * Cons:
      - Operating on a `RuntimeImage` may require switches and exhaustive work.
      - Not an `interface` so inheritance is limited
      - templates were inherently "open-method" at compile-time

- a buffer to render widgets to, is an Image

- Images should support fast subsets (like slicing an array)

- an Image should have named color channels for easy indexing

- Image data should be held by an `IAllocator`

- Images abstractions are 2D, not 3D, not N-dimensional.
    * Pros:
        - simple

    * Cons:
        - some niche domains need higher dimensional images.

## Aspect ratio discussion

There are multiple aspect ratios:
- Source aspect ratio  - shape of the image buffer: `width / height`
- Pixel aspect ratio   - aspect ratio of image pixels as presented on the display
- Display aspect ratio - shape picture should appear on screen: `sourceAspectRatio * pixel_aspect_ratio`

- Various ways to express aspect ratio
    1. Just store `displayAspectRatio`
    2. DPIx:DPIx - `pixelAspectRatio = DPIx / DPIx`
    3. Store `pixelAspectRatio` + `DPIx`

After discussion, we decided to go with #3, because:
1. Just storing `displayAspectRatio` omits DPI information, which can be interesting for certain applications
2. Calculating `pixelAspectRatio` from `DPIx / DPIy` is unreliable, because not all images have DPI information available
    - No reasonable defaults for DPI values, they are just arbitrary
    - If DPI is zero, then `DPIx / DPIy` is division by zero!
3. Storing a `pixelAspectRatio` is convenient, and a reasonable default exists (`1.0`) in lieu
    - It is valid for either pixel aspect, or DPI, or both, or neither may be present
    - Storing a single DPI value is convenient, because most images have matching H/V DPI
    - Lossless transformation between representations is convenient

# Open questions (no vote occured yet)

- Who owns `RuntimeImage` data?

- what exactly owns IAllocator?

- who owns the Image abstraction itself (GC, RC, scoped ownership)?

- how animated Image would be represented?
