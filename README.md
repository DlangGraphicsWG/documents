# Dlang Graphics Workgroup


The output of the Dlang Graphics Workgroup: a technical recommendation for an Image abstraction library, suitable to implement further graphics works.


**Step 1: requirements gathering.**

Please vote on proposals here: https://github.com/DlangGraphicsWG/documents/pulls

- Image: [requirements.md](image.md)
- Image Loading: [requirements.md](image-loading.md)
- Image Resizing: [requirements.md](image-resizing.md)
- Decision process: [decision-process.md](decision-process.md)


# Goals


The Dlang Graphics Workgroup is a collective of D library writers who want to
make libraries together with a collegial design.


We feel that Phobos leads the path towards building reusable, expressive abstractions;
and that there is no reason a GUI Toolkit can't be unopinionated to the same degree std.range or std.allocator is.

We acknowledge that working on our own graphics libraries (like we currently do), may not yield 
the best possible result for everyone; and that discussion and arguing are called 
for if we are to keep the D community engaged.

Because this task is essentially about defining graphical interfaces, and those interfaces exchanges "images",
**our first line of work is the design of an Image library**.

Because this task is essentially about (dis)agreement, we **talk about pain points** (pros/cons)
and give them existence.


## Step 1: 

Find out what we actually need in an Image library.

## Step 2: 

Figure out what we already got that matches it.

## Step 3: 

Figure out what do we need to do to make it fully match our needs.

