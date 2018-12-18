# Requirements
Each item should include pros and cons of why they should and shouldn't be considered valid.
Each item should be written as a use case and not as a technical description of the problem preferably.

## Interfacing to library
- Image that can be a buffer for render widgets
- Animated images (may need a streaming interface)
- Fast subsets of images (like slicing an array)
- Progressive loading (png/jpg for web browsers and such)
- Some way to manipulate an image without knowing or understanding that there is a color (abstract operations)
- Named color channels for easy indexing
- IAllocator and friends to customize and pool of resources

## Internal to library

- Image that can be a buffer for render widgets

- Animated images

- Resizing

  Pros:
      Is needed inside loaders for eg. 4:2:0 loading in JPEG.
  Cons:
      Is an opinionated operation! Many ways to do it.

- Images (and Image operations) are range-like objects that can be composed like in `ae.utils.graphics`

  Pros:
      Friendly and efficient UFCS chains of operations (need good names).
      The Pros of Input ranges, but for images.
  Cons:
      UFCS chains may leave no room to pass a complex context.
      Most of algorithms end up being written for contiguous in-memory images anyway.
      Like `std.range`, functions are all defined externally hence not quite easy to discover.


Unsolved disagreement:
- Images are 1D?
- Images are 2D?
- Images are 3D?
- Images are N-dimensional?

- Image formats should auto-convert to a request color

  pros:
    - Less allocations

  cons:
    - Extra conversions making things less clean

- Custom allocators

- Images types should have a runtime interface

  Pros:
      The main advantage is that you can avoid to template something by the Image type.
      (Like std.allocator have IAllocator so that containers can avoid being templated by the allocators specific type)
      Allows crossing binary interfaces like shared libraries.
      Format conversion may need this internally.
      This "type-punning" reduce template bloat and compile times.
  Cons:
      More work.
      The runtime interface has virtual dispatch.

## External (file formats, that sort of thing)
- Loading+Exporting an image to ubytes
- Animated GIF's
- Lots of C libraries offer pointer to bytes with offset between lines support
- File formats that can be produce small files (like webp, apng, HDR for web)
- File formats with large sample size like EXR
- Advanced file formats to clever things like layer (e.g. EXR)

## Assumptions
- Should not attempt to support video (video is not animated images) as it is too complex and requires a clock for timing
