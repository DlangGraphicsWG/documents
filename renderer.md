## Disclaimer

First of all this is an attempt to gather information about state of Renderer in dlang

# What is a Renderer?
 
Renderer contains a rendering state and provides functionality to draw primitives using specific color or texture. Renderer uses Image as a buffer to render to. Calling renderer method issues command to the backend API. The command is queued and flushed at some later time. Renderer draw the following primitives:
- point
- line
- polyline
- polygone (probably convex only?)
- rectangle
- ellipse
- text
- image

Lines can be stroked, 2D figures can be stroked and filled. Rectange can have rounded corners.

Renderer is 2D only.
Renderer is vector one.
Renderer API is like [dearimgu](https://github.com/ocornut/imgui) or [nuklear](https://github.com/vurtun/nuklear/blob/master/nuklear.h#L28).

Example of renderer: 
- [nanovega](http://dpldocs.info/experimental-docs/arsd.nanovega.html)
- [SDL_Renderer](https://wiki.libsdl.org/SDL_Renderer)
- [cairo](https://www.cairographics.org)
  
  
  # Open questions
   - Is it possible and convinient to have both compile-time and runtime API?
