DLang Graphics Research
=======================
This is an incomplete list of notable libraries related to graphics & ux.
The list contains libraries useful for both creating graphical applications and designing your own graphics framework.


## Building Blocks

### Color
- **std.experimental.color** https://github.com/TurkeyMan/color

- **arsd/color** https://github.com/adamdruppe/arsd/blob/master/color.d

- **dlib/image/color** https://github.com/gecko0307/dlib/blob/master/dlib/image/color.d


### Linear Algebra
- **gfm/math** https://github.com/d-gamedev-team/gfm/tree/master/math/gfm/math

- **gl3n** https://github.com/Dav1dde/gl3n

- **dlib/math** https://github.com/gecko0307/dlib/tree/master/dlib/math


### Numeric
- **sargon/halfloat** https://github.com/DigitalMars/sargon/blob/master/src/sargon/halffloat.d  
  Docs: https://digitalmars.com/sargon/halffloat.html

- **decimal** https://github.com/rumbu13/decimal


### CPU Intrinsics
- **std.simd** https://github.com/TurkeyMan/simd

- **intel-intrinsics** https://github.com/AuburnSounds/intel-intrinsics

- **mir-cpuid** https://github.com/libmir/mir-cpuid


### Constraint Solving & Layout
- **Cassowary** https://overconstrained.io  
  D implementation: https://github.com/yglukhov/cassowary.d



## GUI
- **Qt** https://www.qt.io  
  https://github.com/w0rp/dqt  
  https://github.com/filcuc/dqml  
  https://github.com/MGWL/QtE5

- **wxWidgets** https://www.wxwidgets.org  
  D binding: http://wxd.sourceforge.net

- **GTK** https://www.gtk.org  
  D binding: https://gtkd.org

- **Nanogui** https://github.com/wjakob/nanogui  
  D port: https://github.com/drug007/nanogui

- **Nuklear** https://github.com/vurtun/nuklear  
  D binding: https://github.com/mogud/nukleard  
  D binding: https://github.com/Timu5/bindbc-nuklear

- **Dear ImGui** https://github.com/ocornut/imgui  
  D binding: https://github.com/Extrawurst/DerelictImgui  
  D binding: https://github.com/0dyl/d-cimgui  
  D port: https://github.com/d-gamedev-team/dimgui

- **libui** https://github.com/andlabs/libui  
  D binding: https://github.com/Extrawurst/DerelictLibui  
  D wrapper: https://github.com/mogud/libuid

- **Tcl/Tk** https://www.tcl.tk  
  D binding: https://github.com/nomad-software/tcltk  
  D port: https://github.com/nomad-software/tkd

- **FLTK** https://www.fltk.org

- **Clutter** https://wiki.gnome.org/Projects/Clutter

- **IUP** http://iup.sourceforge.net  
  D binding: https://github.com/carblue/iup  
  D binding: https://bitbucket.org/alphaglosined/libglosined/src/85ab6b2135879848e7efd5f1dfa732f2cfb753f8/iup

- **LittlevGL** https://littlevgl.com

- **Dlang UI** https://github.com/buggins/dlangui

- **DWT** https://github.com/d-widget-toolkit/dwt

- **DQuick** https://github.com/D-Quick/DQuick

- **arsd/minigui** https://github.com/adamdruppe/arsd/blob/master/minigui.d

- **Poison** https://github.com/PoisonEngine/poison-ui

- **ae/ui** https://github.com/CyberShadow/ae/tree/master/ui



## Dialogs
- **Native File Dialog** https://github.com/mlabbe/nativefiledialog

- **tinyfiledialogs** https://github.com/native-toolkit/tinyfiledialogs  
  D port: https://github.com/dayllenger/tinyfiledialogs-d

- **Boxer** https://github.com/aaronmjacobs/Boxer



## Window & Input & Event
- **SDL** https://www.libsdl.org  
  D binding: https://github.com/BindBC/bindbc-sdl

- **GLFW** https://www.glfw.org  
  D binding: https://github.com/BindBC/bindbc-glfw

- **libX11** https://www.x.org  
  D binding: https://github.com/D-Programming-Deimos/libX11

- **wayland-d** https://github.com/rtbo/wayland-d

- **SPEW** https://github.com/Devisualization/spew

- **dplug/window** https://github.com/AuburnSounds/Dplug/tree/master/window/dplug/window

- **arsd/simpledisplay** https://github.com/adamdruppe/arsd/blob/master/simpledisplay.d



## Event-Loop
- **libev** http://software.schmorp.de/pkg/libev.html  
  D binding: https://github.com/D-Programming-Deimos/libev

- **libuv** https://github.com/libuv/libuv  
  D binding: https://github.com/raefaldhia/libuv  
  D binding: https://github.com/tamediadigital/libuv  
  D binding: https://github.com/changloong/libuv

- **libevent** https://libevent.org  
  D binding: https://github.com/D-Programming-Deimos/libevent

- **eventcore** https://github.com/vibe-d/eventcore

- **liuev** https://github.com/troglobit/libuev

- **libasync** https://github.com/etcimon/libasync

- **arsd/eventloop** https://github.com/adamdruppe/arsd/blob/master/eventloop.d

- **SPEW** https://github.com/Devisualization/spew

- **subscribed** https://github.com/v--/subscribed



## Renderer

### High-Level
- **NanoVG** https://github.com/memononen/NanoVG  
  - D port: https://github.com/adamdruppe/arsd/blob/master/nanovega.d  
    - Docs: http://dpldocs.info/experimental-docs/arsd.nanovega.html  
    - Forum: https://forum.dlang.org/thread/yykncywfhvfarrszpqed@forum.dlang.org  
  - D binding & wrapper: https://github.com/Pctg-x8/nanovg-d

- **Cairo** https://www.cairographics.org  
  D binding: https://gtkd.org  
  D wrapper: https://github.com/cairoD/cairoD  
  D binding: https://github.com/D-Programming-Deimos/cairo

- **Skia** https://github.com/google/skia


### Lower-Level
- **OpenGL** https://www.opengl.org  
  D binding: https://github.com/BindBC/bindbc-opengl

- **Vulkan** https://www.khronos.org/vulkan  
  D binding: https://github.com/ParticlePeter/ErupteD

- **DirectX** https://en.wikipedia.org/wiki/DirectX  
  D binding: https://github.com/auroragraphics/directx  
  D binding: https://github.com/evilrat666/directx-d

- **bgfx** https://github.com/bkaradzic/bgfx  
  D binding: https://github.com/GoaLitiuM/bindbc-bgfx



## Image

### Image Loading & Saving
- **FreeImage** http://freeimage.sourceforge.net  
  D binding: https://github.com/BindBC/bindbc-freeimage

- **stb_image** https://github.com/nothings/stb/blob/master/stb_image.h

- **SDL_image** https://www.libsdl.org/projects/SDL_image  
  D binding: https://github.com/BindBC/bindbc-sdl

- **DevIL** https://github.com/DentonW/DevIL  
  D binding: https://github.com/DerelictOrg/DerelictIL

- **libpng** http://www.libpng.org/pub/png/libpng.html  
  D binding: https://github.com/D-Programming-Deimos/libpng

- **ImageMagick** https://imagemagick.org  
  D binding: https://github.com/MikeWey/DMagick

- **arsd/image** https://github.com/adamdruppe/arsd/blob/master/image.d

- **dlib/image** https://github.com/gecko0307/dlib/tree/master/dlib/image


### Image Metadata
- **libexif** https://github.com/libexif/libexif  
  D binding: https://github.com/D-Programming-Deimos/libexif



## Text

### Font Loading & Rasterization
- **FreeType** https://www.freetype.org  
  D binding: https://github.com/BindBC/bindbc-freetype

- **stb_truetype** https://github.com/nothings/stb/blob/master/stb_truetype.h  
  D port: https://github.com/adamdruppe/arsd/blob/master/stb_truetype.d

- **SDL_ttf** https://www.libsdl.org/projects/SDL_ttf  
  D binding: https://github.com/BindBC/bindbc-sdl

- **Glyphy** https://github.com/behdad/glyphy

- **fontconfig** https://www.freedesktop.org/wiki/Software/fontconfig


### Text Shaping & Layout
- **HarfBuzz** https://www.freedesktop.org/wiki/Software/HarfBuzz  
  D binding: https://github.com/DlangGraphicsWG/bindbc-harfbuzz

- **Pango** https://gitlab.gnome.org/GNOME/pango  
  D binding: https://gtkd.org

- **FriBidi** https://github.com/fribidi/fribidi

- **linebreak** https://github.com/s-ludwig/linebreak



## Integration
- **D-Bus** https://www.freedesktop.org/wiki/Software/dbus  
  D wrapper: https://github.com/trishume/ddbus

- **libnotify** https://gitlab.gnome.org/GNOME/libnotify  
  D binding: https://github.com/D-Programming-Deimos/libnotify



## Internationalization (i18n) & Localization (l10n)
- **gettext** https://www.gnu.org/software/gettext

- **djtext** https://github.com/o3o/djtext

- **i18n-d** https://github.com/JakobOvrum/i18n-d

- **mofile** https://github.com/FreeSlave/mofile



## Accesibility
- **ATK** https://gitlab.gnome.org/GNOME/atk  
  D binding: https://gtkd.org



## Uncategorized
- **Allegro5** https://github.com/liballeg/allegro5  
  D binding: https://github.com/DerelictOrg/DerelictAllegro5  
  D binding: https://github.com/SiegeLord/DAllegro5

- **SFML** https://www.sfml-dev.org  
  D binding: https://github.com/DerelictOrg/DerelictSFML2  
  D binding: https://github.com/Jebbs/DSFML

- **ae/utils/graphics** https://github.com/CyberShadow/ae/tree/master/utils/graphics
