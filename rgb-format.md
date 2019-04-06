# RGB format description

## RGB colour properties

Complete set of properties relating to an RGB colour:  
- Components
- Encoding
- Block compression/chroma subsampling strategy
- Gamma compression strategy
- Colour space
- Dynamic range
- Other encoding options
    - Swizzling, Endian, etc

## RGB format string encoding:

`"components_[format]_[colourspace]_["BE" big-endian]_[swizzle parameters]_[userdata]"`

### `components`:  
A set of single character component names:
- `r` `g` `b` - red, green, blue
- `l` - luma/luminance (`r = g = b`)
- `a` - alpha (exempt from colourspace)
- `e` - shared exponent
- `x` - unused bits

Component names are lower-case. (or should they be upper case? they could be case-insensitive, but then we halve the namespace for future expansion)

### `format` [optional]:  
Description of the format for each channel, underscore separated for each component
Eg: `rgb_8_8_8`, `bgra_10_10_10_2`, `rgbe_9_9_9_5`, `rgba_s6_f11_4.8_u3`  
Encoding options:
- Omitted: 8 bit normalised integer for each compoent
- Unsigned normalised integer: `8`, `10`
- Signed normalised integer: `s8`
- Floating point: `f32`, `f11`
- Unsigned fixed point: `1.7`, `8.8`
- Signed fixed point: `s2.6`, `s4.4`
- Unsigned integer: `u16`
- Signed integer: `i16`
- Block compression format: `DXT5`, `ETC1`, `ASTC6x6`
- TODO: chroma subsampling strategy...

Open question: Is a more compact grammar possible? Without separating underscores?

### `colourspace` [optional]:  
Coloursapce name or description.
- Omitted: Assume sRGB
- Typical names: `sRGB`, `DCI-P3`, `BT.709`, (documented set)
- Adapted whitepoint using `'@'`: `sRGB@D50`
- Custom gamma using `'^'`:
    - gamma 2.2: `[colourspace]^2.2`
    - Linear: `[colourspace]^1`
    - Use sRGB hybrid gamma ramp: `[colourspace]^sRGB`
- Use xyY vectors: `{0.312,0.329[,1.0]}`
    - Custom whitepoint: `sRGB@{0.312,0.329}`
    - Custom primaries: `R{0.64,0.33}G{0.3,0.6}B{0.15,0.06}`
- Completely custom colourspace:
    - Eg: `R{0.64,0.33}G{0.3,0.6}B{0.15,0.06}@D50^sRGB`

TODO: Dynamic range is not captured here; but the industry hasn't really settled on standard expressions. We'll need to do a little more research to decide on a good strategy.

Perhaps dynamic range should be separate from colourspace?

### `"BE"` [optional]:  
All formats should be read in bit-order, and encoded as little-endian bit-streams. This marker says to emit the bitstream as big-endian.

It should be noted that this will have the effect of reversing the order of the components when they are byte-aligned, and may appear contrary to a struct with separate element members.

While `rgb_8_8_8` is equivalent to `bgr_8_8_8_BE`, formats where elements straddle byte boundaries require the `BE` specifier to be expressed correctly.

Eg:  
`rrrrrggg gggbbbbb` - `rgb_565`  
`bbbbbggg gggrrrrr` - `bgr_565`  
`gggbbbbb rrrrrggg` - `rgb_565_BE`  
`gggrrrrr bbbbbggg` - `bgr_565_BE`  


### `swizzle parameters` [optional]:  
TODO: develop an expression for swizzling parameters that cover known hardware.  
I think there's no hardware that will fail to be expressed by `[bytes-in-row]:[num-rows]`.

We could use something like `Z32:16` to specify eg: swizzle tiles of 16 rows of 32bytes.

### `userdata` [optional]:  
A string of user data starting with `#`, eg: `#userdata-string`.  
May not contain `_` characters.

## Alternative strategy

I like `rgb_8_8_8` because it's very readable, and the `8_8_8` can be omitted and inferred for the most common colour formats.  
An alternative strategy that eliminates the underscores would be to have the encoding follow the channel immediately, eg: `R8G8B8`  
I feel this becomes very difficult to read with longer formats, eg: `R10fG10fB10fA2`, but it is gramatically sound.

## RGB type

Create an RGB colour struct that can express this set of options...

For inspiration:  
Operable RGB: https://github.com/TurkeyMan/color/blob/master/std/experimental/color/rgb.d  
Bit-packed RGB: https://github.com/TurkeyMan/color/blob/master/std/experimental/color/packedrgb.d
