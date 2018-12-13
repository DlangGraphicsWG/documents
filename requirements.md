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
- If unopinionated must support anything resonable
- Images are 1D arrays
- Images are 2D arrays
- Images are N-Dimensional input/forward ranges
- Be able to change the definition of an image resonably easily (could be 2D to begin with then ND later on)
- Image formats should auto-convert to a request color

  pros:
    - Less allocations

  cons:
    - Extra conversions making things less clean
- Custom allocators

## External (file formats, that sort of thing)
- Loading+Exporting an image to ubytes
- Animated GIF's
- Lots of C libraries offer pointer to bytes with offset between lines support
- File formats that can be produce small files (like webp, apng, HDR for web)
- File formats with large sample size like EXR
- Advanced file formats to clever things like layer (e.g. EXR)

## Assumptions
- Should not attempt to support video (video is not animated images) as it is too complex and requires a clock for timing
