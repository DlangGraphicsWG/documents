## Disclaimer

First of all this is an attempt to gather information about state of Renderer in dlang

# What is a Renderer?
 
Renderer contains a rendering state and provides functionality to draw primitives using specific color or texture. Renderer uses Image as a buffer to render to. Renderer is immediate one, i.e. it instantly modifies underlying Image during calling its methods. Renderer draw the following primitives:
- point
- line
- polyline
- polygone (probably convex only?)
- rectangle
- ellipse
- text
- image

Lines can be stroked, 2D figures can be stroked and filled. Rectange can have rounded corners.

Example of renderer: 
- [nanovega](http://dpldocs.info/experimental-docs/arsd.nanovega.html)
- [SDL_Renderer](https://wiki.libsdl.org/SDL_Renderer)
- [cairo](https://www.cairographics.org)
  
  
  # Open questions
   - What dimensionality should it be? Probably 2D only?
   - Is it possible and convinient to have both compile-time and runtime API?
   - what API should it have?
