# What should be in image loading?


- The Image Loading solution should support turning what is loaded to raw `ubyte[]` data



# Open questions (no vote occured yet)

- A 2-layer architecture for image loading (format decoders + generic interface)

- Error codes or exceptions?

- stance on `-betterC`?

- whether there can be an additional IAllocator in Image loaders for temporary buffers, used for decoding

- Which formats should be supported for loading?

- Which formats should be supported for encoding?

- how much Image loader are supposed to convert from one Color format to another
  Pros:
    - Less allocations
  Cons:
    - Extra conversions making things less clean

- Is video to be supported?

- Should animated format such as GIF be supported?

- Should progressive images such as GIF be supported?

- Progressive images (such as progressive JPEG) should be supported with an incomplete input?

- What happens when loading something with another colorspace as sRGB? should it auto-convert?

- Whether Image loaders should have short-paths for getting image meta-data such as dimensions

- What meta-data is accessible to the visible API?
