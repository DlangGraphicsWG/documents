# The simplest low-level CPU rasterization library

This document discusses a minimal low-level API and reference implementation of 2D rasterizer on CPU.

The CPU backend is useful as a reference implementation and as a fallback, when no GPU is available. Therefore it has to be simple and reasonably fast.

The rasterizer is a quite independent module. It doesn't know about the target surface, colors, etc.

We need to be able to draw various shapes: from simple rectangles to complex filled and stroked paths. They can be converted into primitives such as triangles. If there is an efficient method to rasterize complex shapes, we can avoid conversion costs and render them directly.

These primitives are:
- trapezoids with horizontal top and bottom edges
- thin one-pixel lines
- polygons with several contours and a fill rule

Trapezoids are simpler and faster on CPU than triangles, and more suitable for 2D GUI. They are used in [Cairo and XRender](http://pixman.org/) for decades. A lot of [triangulation algorithms](https://en.wikipedia.org/wiki/Polygon_triangulation) construct trapezoid chains as a first step.

Trapezoids are better to antialias, because, when `y` coordinates are not fractional, it only needs to smooth side edges, which are (almost?) always outside a shape.

Thin lines could be represented as thin trapezoids, but it's more efficient to use [Bresenham's](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm) and [Wu's algorithms](https://en.wikipedia.org/wiki/Xiaolin_Wu%27s_line_algorithm).

Polygons are efficiently rasterized using [scanline filling algorithms](https://nothings.org/gamedev/rasterize/). Transformed shapes (such as a rotated rectangle) are harder to split into trapezoids and better to rasterize as a whole.

## How to represent common shapes using these primitives

Assume we can flatten Bézier curves and arcs, i.e. make a list of segments with some precision.

Rectangle - a trapezoid with vertical sides.

Triangle - the simplest polygon.

Circle, Ellipse - flatten the left arc, construct symmetrical trapezoids from top to bottom.

Convex, [y-monotone](https://en.wikipedia.org/wiki/Monotone_polygon) polygons - split into trapezoids in *O(n)* time with a simple scan-conversion algorithm (it is faster than drawing polygons directly).

Non-convex, self-intersecting polygons - all handled by the polygon rasterizer.

Filled path - flatten contours, rasterize polygons.

Basic 1px line - trivially a thin line.

Stroke with 1px width - flatten it to get a list of thin lines.

Stroke with <1px width - fade it and set its width to 1px.

Stroke with >1px width, unevenly scaled stroke - flatten it, expand into a polygon, rasterize it.

Font glyphs - flatten, rasterize polygons.

## API

For simplicity, this API is not templated. It uses `interface` and therefore virtual calls. It doesn't accept generic ranges also.

```D
struct Box
{
    int x, y, w, h;
}
struct Point
{
    float x, y;
}
struct HorizEdge
{
    float l, r, y;
}

enum Antialiasing
{
    none,
    fast,
    good,
}
enum FillRule
{
    nonzero,
    odd,
    zero,
    even,
}

struct Params
{
    Antialiasing aa;
    Box clip;
    FillRule fillRule;
}
interface Plotter
{
    void setPixel(int x, int y);
    void mixPixel(int x, int y, float coverage);
    void setScanLine(int x0, int x1, int y);
    void mixScanLine(int x0, int x1, int y, float coverage);
}

void rasterizePolygons(const Point[], const uint[] contours, Params, Plotter);
void rasterizeTrapezoidChain(const HorizEdge[], Params, Plotter);
void rasterizeLine(Point, Point, Params, Plotter);
```

All geometry data is `float` for consistency.

Each function performs clipping by an integer rectangle. It is necessary because results must not exceed the surface bounds. `clip` is an intersection of the image boundaries and some clipping box.

`zero` and `even` fill rules are complementary to `nonzero` and `odd`. They are useful for clipping.

Trapezoid chains are better than individual trapezoids because the latter requires a mask for correct antialiasing where edges join.

## Questions

## Links

* [pixman - low-level software library for pixel manipulation](http://pixman.org/)
* [Polygon triangulation](https://en.wikipedia.org/wiki/Polygon_triangulation)
* [Monotone polygon](https://en.wikipedia.org/wiki/Monotone_polygon)
* [How the stb_truetype rasterizer works](https://nothings.org/gamedev/rasterize/)
* [Triangulating Simple Polygons and Equivalent Problems](https://www.researchgate.net/publication/220183456_Triangulating_Simple_Polygons_and_Equivalent_Problems)
* [Bresenham's line algorithm](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm)
* [Xiaolin Wu's line algorithm](https://en.wikipedia.org/wiki/Xiaolin_Wu%27s_line_algorithm)
