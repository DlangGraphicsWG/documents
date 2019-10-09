# The simplest low-level CPU rasterization library

This document discusses a minimal lowest-level API and reference implementation of 2D geometry renderer on CPU.

We need to be able to draw various shapes: from simple rectangles to complex filled and stroked paths.
All these shapes can be converted into simpler ones - primitives - to be efficiently rasterized with a minimum of code.

These are:
- trapezoids with horizontal top and bottom edges
- thin one-pixel lines

Trapezoids are simpler and faster on CPU than triangles, and more suitable for 2D GUI. They are used in [Cairo and XRender](http://pixman.org/) for decades. A lot of [triangulation algorithms](https://en.wikipedia.org/wiki/Polygon_triangulation) construct trapezoid chains as a first step.

Trapezoids are better to antialias, because, when `y` coordinates are not fractional, it only needs to smooth side edges, which are (almost?) always outside a shape.

Also, to draw trapezoids on GPU is only to split them in triangle pairs, so reference GPU backend can be implemented quicker using existing trapezoidation code (though of course it is better to write specialized triangulators later).

Thin lines could be represented as thin trapezoids, but it's more efficient to use [Bresenham's](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm) and [Wu's algorithms](https://en.wikipedia.org/wiki/Xiaolin_Wu%27s_line_algorithm).

## How to represent common shapes using these primitives

Assume we can flatten Bézier curves and arcs, i.e. make a list of segments with some precision.

Rectangle - a trapezoid with vertical sides.

Triangle - either one or two trapezoids with one zero-length base.

Circle, Ellipse - flatten the left arc, construct symmetrical trapezoids from top to bottom.

Other convex polygons - any [y-monotone polygon](https://en.wikipedia.org/wiki/Monotone_polygon) can be split into trapezoids in *O(n)* time with a simple scan-conversion algorithm.

Non-convex simple polygons, polygons with holes - a first step of several triangulation algorithms.

Non-convex polygons with self-intersections - there are algorithms to divide such polygons into simple ones.

Filled path - flatten contours, merge them applying fill rules to get a polygon.

Basic 1px line - trivially a thin line.

Stroke with 1px width - flatten it to get a list of thin lines.

Stroke with <1px width - fade it and set its width to 1px.

Stroke with >1px width, unevenly scaled stroke - flatten it, construct a tristrip chain, convert it to trapezoids.

## API

```D
struct Trapezoid
{
    TrapezoidBase top, bottom;
}
struct TrapezoidBase
{
    float left, right, y;
}

alias Plot = void delegate(int x, int y);
alias PlotAA = void delegate(int x, int y, float coverage);

void drawTrapezoids(const Trapezoid[], Plot);
void drawTrapezoidsAA(const Trapezoid[], Plot, PlotAA);

void drawLine(int x0, int y0, int x1, int y1, Plot);
void drawLineAA(float x0, float y0, float x1, float y1, PlotAA);
```

## Questions
- plotting function can be passed as `alias plot`; it should be faster, but I guess it complicates API for now
- `int` or `float` coordinates for non-antialiasing functions?

## Links

* [pixman - low-level software library for pixel manipulation](http://pixman.org/)
* [Polygon triangulation](https://en.wikipedia.org/wiki/Polygon_triangulation)
* [Triangulating Simple Polygons and Equivalent Problems](https://www.researchgate.net/publication/220183456_Triangulating_Simple_Polygons_and_Equivalent_Problems)
* [Fast Polygon Triangulation based on Seidel's Algorithm](https://www.cs.unc.edu/~dm/CODE/GEM/chapter.html)
* [Triangulation and shape complexity](https://www.cs.princeton.edu/~chazelle/pubs/TriangulShapeComplexity.pdf)
* [Bresenham's line algorithm](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm)
* [Xiaolin Wu's line algorithm](https://en.wikipedia.org/wiki/Xiaolin_Wu%27s_line_algorithm)
