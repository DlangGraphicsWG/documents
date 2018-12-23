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
 - [dearimgu](https://github.com/ocornut/imgui)
 - [nuklear](https://github.com/vurtun/nuklear/blob/master/nuklear.h#L28).
