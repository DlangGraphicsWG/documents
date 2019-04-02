Sumary of Cairo, PIXMAN, Skia, OpenGL, pixel formats + some random thoughts.


cairo formats
=============

CAIRO_FORMAT_INVALID --- no such format exists or is supported.

CAIRO_FORMAT_ARGB32 --- each pixel is a 32-bit quantity, with alpha in the upper 8 bits, then red, then green, then blue. The 32-bit quantities are stored native-endian. Pre-multiplied alpha is used. (That is, 50% transparent red is 0x80800000, not 0x80ff0000.) (Since 1.0)

CAIRO_FORMAT_RGB24 --- each pixel is a 32-bit quantity, with the upper 8 bits unused. Red, Green, and Blue are stored in the remaining 24 bits in that order. (Since 1.0)

CAIRO_FORMAT_A8 --- each pixel is a 8-bit quantity holding an alpha value. (Since 1.0)

CAIRO_FORMAT_A1 --- each pixel is a 1-bit quantity holding an alpha value. Pixels are packed together into 32-bit quantities. The ordering of the bits matches the endianness of the platform. On a big-endian machine, the first pixel is in the uppermost bit, on a little-endian machine the first pixel is in the least-significant bit. (Since 1.0)

CAIRO_FORMAT_RGB16_565 --- each pixel is a 16-bit quantity with red in the upper 5 bits, then green in the middle 6 bits, and blue in the lower 5 bits. (Since 1.2)

CAIRO_FORMAT_RGB30 --- like RGB24 but with 10bpc. (Since 1.12)

notes:

channel and pixel order is dependent on machine endianness.
the RGB24 is actually 32 bits per pixel,

https://cairographics.org/manual/cairo-Image-Surfaces.html


OpenGL
======

Splits format into component list and data type

data types

normalized integers, signed and unsigned
floating point
integer, signed and unsinged

bit depths supported for each type...

unsigned normalized (no suffix)	21, 42, 53, 8, 103, 122, 16
signed normalized	8, 16
unsigned integral	8, 16, 32
signed integral	8, 16, 32
floating point	16, 32

components format is limited to

"R", "RG", "RGB", or "RGBA"; 

some specialised formats that dont fit the above syntax...

GL_R3_G3_B2: Normalized integer, with 3 bits for R and G, but only 2 for B.
GL_RGB5_A1: 5 bits each for RGB, 1 for Alpha
GL_RGB10_A2: 10 bits each for RGB, 2 for Alpha
GL_R11F_G11F_B10F: This uses special 11 and 10-bit floating-point values.
GL_RGB9_E5: This one is complicated. It is an RGB format of type floating-point. 
GL_RGB565: 5 bits each for RB, 6 for G.

wikipedia page doesnt say anything about byte order / endianness

notes:

There's more than what i've listed tbh, depth formats, and other stuff.

https://www.khronos.org/opengl/wiki/Image_Format


PIXMAN
======

has the following pixel types...

#define PIXMAN_TYPE_OTHER	0
#define PIXMAN_TYPE_A		1
#define PIXMAN_TYPE_ARGB	2
#define PIXMAN_TYPE_ABGR	3
#define PIXMAN_TYPE_COLOR	4
#define PIXMAN_TYPE_GRAY	5
#define PIXMAN_TYPE_YUY2	6
#define PIXMAN_TYPE_YV12	7
#define PIXMAN_TYPE_BGRA	8
#define PIXMAN_TYPE_RGBA	9
#define PIXMAN_TYPE_ARGB_SRGB	10
#define PIXMAN_TYPE_RGBA_FLOAT	11

then you can define bits per pixel and bits per component, it has a macro that defines the actual enum value so these properties are encoded in the enum...

24..31 = bits per pixel
16..23 = pixel type
0..3,4..7,8..11,12..15 = bits used for each component

then actual format enums are encoded thus...

    PIXMAN_a8r8g8b8 =	 PIXMAN_FORMAT(32,PIXMAN_TYPE_ARGB,8,8,8,8),

there's a seperate macro that handles float format since it the main one can only go up to 16 bits per component.

cant find any info regarding endianness of format

https://github.com/servo/pixman/blob/master/pixman/pixman.h


Skia
====

skia defines alpha type...

    kUnknown_SkAlphaType,
    kOpaque_SkAlphaType,
    kPremul_SkAlphaType,
    kUnpremul_SkAlphaType,

and components type

    kUnknown_SkColorType,
    kAlpha_8_SkColorType,
    kRGB_565_SkColorType,
    kARGB_4444_SkColorType,
    kRGBA_8888_SkColorType,
    kRGB_888x_SkColorType,
    kBGRA_8888_SkColorType,
    kRGBA_1010102_SkColorType,
    kRGB_101010x_SkColorType,
    kGray_8_SkColorType,
    kRGBA_F16_SkColorType,
    kRGBA_F32_SkColorType,
    kLastEnum_SkColorType = kRGBA_F32_SkColorType,
    kN32_SkColorType = kBGRA_8888_SkColorType,
    kN32_SkColorType = kRGBA_8888_SkColorType,

from what I can understand pixels are considered as machine words, ie 16 bits per pixel is considered as a uint16, 64 bits per pixel is a uint64, and the channels are parsed like hex literal, first channel is most significant bits, IE...

ARGB32, is like 0xAARRGGBB, 

So i guess byte order depends on endieness of the platform, but its not very clear.
notes..

First library to mention premultiplied alpha i think

Doesnt mention and formats that are less that 8 bits per pixel so doesnt have anything to say about ordering for when multiple pixels are encoded into a byte.

https://skia.org/user/api/SkImageInfo_Reference#SkImageInfo


=============================================================

thoughts....

Useful to have an 'X' component for unused bits, EG..
XRGB32, top 8 bits are unused.

component encodings...
unsigned & signed int, various widths
normalized, signed and unsigned int, various widths
float formats, various widths.
OpenGL has some weird formats, 9,11 bit floats.

Desirable to be able to define both plain alpha and premultiplied alpha.

Will have to define  A,X,R,G,B, something for Gray? What is "YUY2" in pixman list?

'P' for premultiplied alpha?

so PRGB vs ARGB?

What about indexed formats? EG somthing like.. "I8" = indexed 8 bits per pixel??

Need to define how how format strings relates to bit/byte ordering. 
The common way to relate component string to ordering is to think of it as hex literal ordering IE...

ARGB32 ==> 0xAARRGGBB,
so on little endian Alpha is high bits of 32 bit word 
so on big endian Alpha is low bits of 32 bit word 

But better to not depend on endieness of system imo.

also need to consider that where more than one pixel is encoded into a byte the ordering may be different, see CAIRO_FORMAT_A1

Could have a shorthand format for when bits per component is equal...

ARGB32, 8 bits each component
RGB24, 8 bits each
XRGB16, 4 bits each, top 4 bits unused
RGB48f, 16 bit float for each

then long form for where that wont work

ARGB_2_10_10_10, 2 alpha, 10 each of the rest
RGB_5_6_5, 5 red, 6 green, 5 blue

long form if you need the weird OpenGL stuff..

RGB_10un_12un_10un, 10 bits usigned normalized.. etc... 
