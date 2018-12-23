## Disclaimer

First of all this is an attempt to gather information about state of Renderer in dlang

# What is a Draw Command Buffering API?

This API provides functionality to draw primitives issuing draw commands that will be performed some time later using the backend API. In fact it is a layer between generic part of renderer independent on hardware and hardware specific part of renderer. The draw command buffer API lets draw the following primitives:
- point
- line
- polyline
- polygone (probably convex only?)
- rectangle
- ellipse
- text
- image

Lines can be stroked, 2D figures can be stroked and filled. Rectange can have rounded corners.

API is 2D only.
API is vector one.
Example of draw command buffer API:
 - [dearimgu](https://github.com/ocornut/imgui/blob/2889a14f86823c4b39b6e4d070fef19064167ab2/imgui.h#L1817)
 - [nuklear](https://github.com/vurtun/nuklear/blob/181cfd86c47ae83eceabaf4e640587b844e613b6/nuklear.h#L4701).

# Sample API

Based on dearimgui and nuklear APIs

```D
enum Stroke { open = false, closed = true }
enum AntiAliasing { off, on }

// Path API
path_clear(Ctx);
path_line_to(Ctx, Vec2 pos);
path_arc_to(Ctx, Vec2 center, float radius, float a_min, float a_max, uint segments);
path_rect_to(Ctx, Vec2 a, Vec2 b, float rounding);
path_curve_to(Ctx, Vec2 p2, Vec2 p3, Vec2 p4, uint num_segments);
path_fill(Ctx, Color);
path_stroke(Ctx, Color, Stroke closed, float thickness);

// stroke
stroke_line(Ctx, Vec2 a, Vec2 b, Color, float thickness);
stroke_rect(Ctx, rect, Color, float rounding, float thickness);
stroke_triangle(Ctx, Vec2 a, Vec2 b, Vec2 c, Color, float thickness);
stroke_circle(Ctx, Vec2 center, float radius, Color, uint segs, float thickness);
stroke_curve(Ctx, Vec2 p0, Vec2 cp0, Vec2 cp1, Vec2 p1, Color, uint segments, float thickness);
stroke_poly_line(Ctx, ref const(Vec2) pnts, uint cnt, Color, Stroke, float thickness, AntiAliasing aa);

// fill
fill_rect(Ctx, rect, Color, float rounding);
fill_triangle(Ctx, Vec2 a, Vec2 b, Vec2 c, Color);
fill_circle(Ctx, Vec2 center, float radius, Color col, uint segs);
fill_poly_convex(Ctx, ref const(Vec2) pnts, uint count, Color, AntiAliasing aa);

// image
add_image(Ctx, Image texture, rect, Color);

// text
add_text(Ctx, ref Font, Rect, string text, float font_height, Color);

// scissor
scissor(Ctx, Rect);
```

Notes:
- deargui has ability to cancel previous scissor but it is just sugar
- Dearimgui provides additional functionality - channels.
- Nuklear and dearimgui have close implemention. Path stroking lowered to polyline drawing with or without antialiasing with very similar implementation details. For example nanovega has very different implementation details but probably it is because tightly coupling with opengl backend. dearimgui and nuklear are much more backend agnostic, especially nuklear.

