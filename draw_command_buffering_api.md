## Disclaimer

First of all this is an attempt to gather information about state of Renderer in dlang

# What is a Draw Command Buffering API?

This API provides functionality to draw primitives issuing draw commands that will be performed some time later using the backend API. In fact it is a layer between generic part of renderer independent on hardware and hardware specific part of renderer. The draw command buffer API lets draw the following primitives:
- point
- line
- polyline
- polygone (convex only)
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

There are many ways to implement draw command buffer that do same things in different and non-interchangeble ways. For example `dearimgui` and `nuklear` implements draw command buffer in very similar but non API compatible ways. This document try to define one of possible variants based on `dearimgui` and `nuklear` APIs

```D
enum Stroke { open = false, closed = true }
enum AntiAliasing { off, on }

// Path API
pathClear(Ctx);
pathLineTo(Ctx, Vec2 pos);
pathArcTo(Ctx, Vec2 center, float radius, float a_min, float a_max, uint segments);
pathRectTo(Ctx, Vec2 a, Vec2 b, float rounding);
pathCurveTo(Ctx, Vec2 p2, Vec2 p3, Vec2 p4, uint num_segments);
pathFill(Ctx, Color);
pathStroke(Ctx, Color, Stroke closed, float thickness);

// stroke
strokeLine(Ctx, Vec2 a, Vec2 b, Color, float thickness);
strokeRect(Ctx, rect, Color, float rounding, float thickness);
strokeTriangle(Ctx, Vec2 a, Vec2 b, Vec2 c, Color, float thickness);
strokeCircle(Ctx, Vec2 center, float radius, Color, uint segs, float thickness);
strokeCurve(Ctx, Vec2 p0, Vec2 cp0, Vec2 cp1, Vec2 p1, Color, uint segments, float thickness);
strokePolyLine(Ctx, ref const(Vec2) pnts, uint cnt, Color, Stroke, float thickness, AntiAliasing aa);

// fill
fillRect(Ctx, rect, Color, float rounding);
fillTriangle(Ctx, Vec2 a, Vec2 b, Vec2 c, Color);
fillCircle(Ctx, Vec2 center, float radius, Color col, uint segs);
fillPolyConvex(Ctx, ref const(Vec2) pnts, uint count, Color, AntiAliasing aa);

// image
addImage(Ctx, Image texture, rect, Color);

// text
addText(Ctx, ref Font, Rect, string text, float font_height, Color);

// scissor
scissor(Ctx, Rect);
```

Notes:
- `Dearimgui` has ability to cancel previous scissor but it is just sugar
- `Dearimgui` provides additional functionality - channels.
- `Nuklear` and `dearimgui` have close implemention. Path stroking lowered to polyline drawing with or without antialiasing with very similar implementation details. For example `nanovega` has very different implementation details but probably it is because tightly coupling with opengl backend. `dearimgui` and `nuklear` are much more backend agnostic.

## Open questions:
1. context+standalone functions (`nuklear`, plain c) vs aggregate with methods (`dearimgui`, C++)? there is no big difference I guess but nevertheless
2. class vs struct? reference type vs value type
3. to use const qualifier or not to use?
4. polygons are convex only (at least now) - both `dearimgui` and `nuklear` use only convex ones
5. coordinates are fractional. In `dearimgui` pixels have fractional coordinates but width and height for example are integer values. `Nuklear` has float coordinates and also width and height.
6. how to set Image where the backend render to? both `dearimgui` and `nuklear` have no this functionality, they render to default buffer(?). Probably it could be implemented using:
```
ctx.beginFrame(Image image);
...
ctx.endFrame;
```
7. Vec2, Rect, Color - is it hardcoded types or templated API possible? `nuklear` let you define your own pixel/vertex format, so it is claimed by users.

# Fonts and text drawing

Planned to some next PR.
