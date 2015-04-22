---
layout:     page
title:      "Deferred Shading Engine"
section:	index
style:		dark
---

### A Rendering Engine written in C++ and OpenGL

![The Rungholt scene rendered with simple diffuse lighting and SSAO](../images/deferred.png)
*The Rungholt scene rendered with diffuse lighting, SSAO, and shadow mapping*

### Overview ###
This rendering engine, which is still under development, is being used in "Array", an upcoming 3D puzzle-action game for Cornell's Advanced Game Design class. I am also using this as an opportunity to explore what it takes to create a complete rendering engine from scratch. The goal for this project is to create an engine capable of a toon-shading style with advanced post-processing effects and support for various light volumes while still being performant enough to support a real-time game.

<pre></pre>
The code is also available at my [github repository](https://github.com/JAGJ10/DeferredRenderer).

### Features ###
- **Deferred Pipeline** - Geometry is rendered into buffers in the first pass (position, normals, diffuse color, and depth) which allows the subsequent passes to only calculate lighting for the currently visible objects. This is the main advantage of deferred rendering compared to forward rendering where the lighting needs to be calculated on every object even if they are not visible.
- **Octree** - A data structure used for spatial partitioning which culls objects not currently in the view area. This decreases the amount of geometry that needs to be drawn each frame, greatly increasing performance.
- **Screen-Space Ambient Occlusion** - Using the hemisphere variant of the technique developed by Crytek, a cheap ambient occlusion calculation is made before lighting is computed. This gives a greater sense of depth by making "occluded" areas darker.
- **Light Volumes** - Point lights and spot lights are represented by meshes which allows the lighting calculations to only be performed on pixels within the affected area. This greatly decreases computational costs which is important when many lights are in the scene.
- **Shadow Mapping** - Shadow maps are calculated for each light in the scene at half resolution (with the exception of the directional light which will have a larger resolution for quality) to generate shadows in the scene. Since shadow mapping is a relatively cheap operation, it will be possible to do this even for a large amount of light geometry. Additionally, Percentage Closer Filtering is used to generate softer shadows for the edges.
- **Depth of Field (under development)** - This effect will blur the part of the screen that is not centered, giving a kind of "focus" to what the camera is currently looking at.
- **Bloom (under development)** - By making a copy of the current frame and slightly blurring it, a simple bloom effect can be achieved. This will make the bright areas "bleed" over into other areas which enhances the realism of the scene. Even with toon-shading, this feature can have a pleasant effect as seen in the Wii U version of The Legend of Zelda - Wind Waker.

### Acknowledgements ###
I would like to personally thank Professor [Steve Marschner](http://www.cs.cornell.edu/~srm/) for his help and constant willingness to answer any and all questions.

### Screenshots ###
![The Rungholt scene rendered with simple diffuse lighting and SSAO](../images/ssao.png)
*The result of the SSAO pass in the same scene as above (from a different angle)*
![The Rungholt scene rendered with simple diffuse lighting and SSAO](../images/deferred2.png)
*SSAO combined with 50 randomly generated point lights placed throughout the scene with various colors*

### Resources ###
Almost all of the resources used thus far have been tutorials and forums online. Since these are too numerous to list, I encourage anyone who is interested in learning more about deferred shading to start from the ground up and try creating their own. Throughout development I have found that there is no "correct" way of implementing the pipeline and that it is all depenedent on what the engine is being used for.