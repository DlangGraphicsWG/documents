# PNG Read/Write

- Should we call it reader or decoder?
- Should we call it writer or encoder?

For the rest of the document I will call them reader and writer. I only implemented parts of the reader for now so some things from bellow will definitely change a bit in order to support the writer as well.

## Structure


- wg/format/png
	- common.d
	- reader.d
	- writer.d

##### Common.d

It contains code common for reader and writer: structs that mirror the file records or are part of both APIs, definitions of chunk types and similar. The main API data structure is this:

```d
struct PngImage
{
    PngDataStatus status;
    string error;
    // Linked list of chunk data: char[4] type, ubyte[] data. Contains PLTE and ancillary chunks.
    PngChunk* chunk;
    ubyte[] pixels;
    PngHeaderData info;
    alias info this;
}

enum PngImageStatus
{
    empty,
    infoLoaded, // Only the header data is loaded
    dataLoaded, // All data is loaded
    errorEncountered
}

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

// This is used to specify the desired format of pixels after they are read from file and processed
enum PngFormat {
    raw = 0,
    // The lower 4 bits tells how much bytes each format require per pixel
    gray = 1,
    grayAlpha = 2,
    rgb = 3,
    rgba = 4,
    rgb16 = 6,
    rgba16 = 8,
    gray16 = 0x12,
    grayAlpha16 = 0x14
}
```

##### Reader.d

API are the following functions:

```d
// Loads only the header info into the PngImage struct. Doesn't need allocator.
PngImage loadPngHeader(ref ubyte[] file) nothrow @nogc;

// Loads whole image into PngImage struct.
PngImage loadPng(ref ubyte[] file, PngFormat desiredFormat = PngFormat.rgba);
PngImage loadPng(ref ubyte[] file, Allocator* allocator, PngFormat desiredFormat = PngFormat.rgba) nothrow @nogc;
```
File array is given as ref because these functions will move it behind the read data which might be useful if loading from pak file or something like that. In case you called `loadPngHeader` and now have PngImage with only header loaded and you want to load its data you can use one of these:
```d
struct PngImage {
    ...
    // They return false on error but also update the status and error string in the structure
    bool loadData(ref ubyte[] file, PngFormat desiredFormat = PngFormat.rgba);
    bool loadData(ref ubyte[] file, Allocator* allocator, PngFormat desiredFormat = PngFormat.rgba) nothrow @nogc
}
```


## Open Questions

1. Supporting interlaced (progressive) format would bring in a lot of complexity, would probably require more API changes and it is very rare these days. Would we be ok with the decision not to support it until further notice? (Disclaimer: I am not implementing it either way :))
2. Should we make a separate float gamma field in PngImage struct that gets populated from gAMA chunk automatically instead of having to look for it in chunk data linked list?
3. Should we even use gamma from the file as given? This [article](https://hsivonen.fi/png-gamma/) speaks of problems with it.
4. Should we add pixelFormat field that says in which format pixels array is, or is it enough that the caller knows what format he asked for?
5. Should the package provide API with File and be made to work by loading part of the file at a time, instead of loading the whole file in memory before processing? This would add a solid amount of complexity...
6. Should PngImage struct save the reference to the allocator and provide a `free` method that frees all the data it allocated from it (pixels and chunks linked list)?
7. I assume most of the time the caller will want to just keep the pixels array and discard everything else so by default we shouldn't even allocate and load chunks linked list, but how do we then allow user to say "I do want chunk data and I will take care of deallocating it when needed"? Maybe also add functions like loadDataWithAllChunks(...) to the API?
8. I decided to use one mmap/VirtualAlloc call (through MMapAllocator) to allocate all the memory I will need for temporary processing while I load and process the data. Will this be good enough for all use cases?
9. Having desiredFormat in the API minimizes allocations and transformations needed to come to the final result but do we want to provide the transformation methods separately instead, at the cost of having to allocate multiple times?
10. One specific transformation requires some color space magic: rgb to gray. Should we:
	1. make this package depend on color space stuff in order to provide this,
    2. just put the code needed for this one transformation in this package directly or
    3. just say that rgb to gray and gray to rgb transformations are not supported by this package and you need to do it using a higer level API like ImageBuffer? ImageBuffer will probably need to support this either way so we could use that to keep this package simpler.
