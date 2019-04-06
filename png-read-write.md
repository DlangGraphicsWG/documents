# PNG Read/Write

I only implemented parts of the reader for now so some things from bellow will definitely change a bit in order to support the writer as well.

## Structure


- wg/format/png
	- common.d
	- reader.d
	- writer.d

##### Common.d

It contains code common for reader and writer: structs that mirror the file records or are part of both APIs, definitions of chunk types and similar. The main API data structure is this:

```d
struct Result(T) {
    string errorMessage;
    T data;
}

Result!PngHeaderData loadPngHeader(const ubyte[] data);
Result!ImageBuffer loadPng(const ubyte[] data, Allocator* allocator)
Result!ImageBuffer loadPng(const ubyte[] data, Allocator* allocator, Allocator* chunkAllocator, out PngChunks* chunks)


// This structure mirrors the file record
align(1)
struct PngHeaderData
{
    uint width;
    uint height;
    PngBitDepth bitDepth;
    PngColorType colorType;
    PngCompressionMethod compressionMethod;
    PngFilterMethod filterMethod;
    PngInterlaceMethod interlaceMethod;
}
```

##### Reader.d

With the above API you can read just the headers or the whole image. The ImageBuffer would contain raw pixel data from the file and all chunks including the IHDR would be included in metadata. In case raw pixel data is in color type 3 (palette indices), it would actually be depalettized into RGB or RGBA if transparency chunk is also present but those two chunks would still be present in the metadata. The ImageBuffer format could thus end up being:

- l, l_1, l_2, l_4, l_16 for grayscale images without alpha and without transparency
- la, la_1_1, la_2_2, la_4_4, la_16_16 for grayscale images with alpha or with transparency
- rgb, rgb_16 for palette images without transparency and rgb images
- rgba, rgba_16 for palette images with transparency and rgba images

Any of these might get "^<g>" where <g> is non standard gamma read from the gamma chunk and "R{x,y,Y}G{x,y,Y}B{x,y,Y}" if there is a cHRM chunk. I have no idea what, if anything, should the reader do with sRGB and iCCP chunks if they are present.

The reader should support interlaced images but should not be complicated with a streaming API (where png file is received in parts and read in multiple passes). It also should not be complicated with File API, but assume the png file can always be completely loaded in memory before parsing.

If user wants to get ImageBuffer in certain format he will have to load one ImageBuffer and then through conversion functions get another ImageBuffer. This will require additional allocation but it makes the reader code and API much simpler.

We are ok with PngChunks being implemented however for the first version and we can separately discuss later how to improve them, since that goes under "solving the memory ownership and auto allocation and deallocation" part which we need to solve for everything anyway.

## Open Questions

1. Is my interpretation on what to do with format string above correct?
2. Should the API allow loading only some chunks and not just all or nothing?
3. I decided to use one mmap/VirtualAlloc call (through MMapAllocator) to allocate all the memory I will need for temporary processing while I load and process the data. Will this be good enough for all use cases?
