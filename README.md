﻿-------------------------------------------------------------------------------
CIS565: Project 4: CUDA Rasterizer
-------------------------------------------------------------------------------
Fall 2014
-------------------------------------------------------------------------------
Due Monday 10/27/2014 @ 12 PM
-------------------------------------------------------------------------------

-------------------------------------------------------------------------------
INTRODUCTION:
-------------------------------------------------------------------------------
This project is a simplified CUDA based implementation of a standard rasterized graphics pipeline, similar to the OpenGL pipeline. 

Basic Features Implemented:
* Vertex Shading
* Primitive Assembly with support for triangle VBOs/IBOs
* Perspective Transformation
* Rasterization through either a scanline or a tiled approach
* Fragment Shading
* A depth buffer for storing and depth testing fragments
* Fragment to framebuffer writing
* A simple lighting/shading scheme, such as Lambert or Blinn-Phong, implemented in the fragment shader

Extra Features:
* Displacement Mapping
* Blending
* Correct color interpolation between points on a primitive
* Texture mapping
* Support for additional primitices. Each one of these can count as HALF of a feature.
   * Points
* MOUSE BASED interactive camera support. Interactive camera support based only on the keyboard is not acceptable for this feature.
	Left button - rotate
	Right button - zoom in/out

Video Link : https://www.youtube.com/watch?v=JLsA9GvFxMs

![alt tag](https://github.com/zxm5010/Project4-Rasterizer/blob/master/random_render.jpg)
-------------------------------------------------------------------------------
THIRD PARTY SOFTWARE
-------------------------------------------------------------------------------

bitmap_image.hpp - A simple bitmap reader that reads BMP files.

-------------------------------------------------------------------------------
PERFORMANCE EVALUATION
-------------------------------------------------------------------------------
According to the round table, we could observe that it took much longer time in the pipeline of rasterization and fragment shader. The reason for that is that we have an atomic function in the rasterization to make sure that we only store nearest primitives into depth buffer. The waiting time for other threads to access same memory costs a lot.
![alt tag](https://github.com/zxm5010/Project4-Rasterizer/blob/master/pipeline_timing.jpg)

Secondly, as we can see from table 1, the performance of rasterization dramastically decrease if the primitives are closer to the view port or are larger, which is because each primitive checks with more pixels it overlaps. To increase the performance, we could use polygon fill method hierarchically instead of scanline in Rasterization pipeline. 

![alt tag](https://github.com/zxm5010/Project4-Rasterizer/blob/master/table1.jpg)


Finally, I tested rasterizer with two meshes, bunny and dragon. The bunny has 2503 vertices and 4968 faces, and dragon has 50000 Vertices and 1000000 Faces. According to table 2, the rasterization for dragon took longer time on displacement, vertex shader, primitive assembly stage due to higher frequency on global memory access. For rasterization and fragment, the atomic function took the majority latency. As the result there is not a huge different. 

![alt tag](https://github.com/zxm5010/Project4-Rasterizer/blob/master/table3.jpg)

-------------------------------------------------------------------------------
Other results
-------------------------------------------------------------------------------
Alpha Blending:
![alt tag](https://github.com/zxm5010/Project4-Rasterizer/blob/master/alpha_blending.jpg)

Points Rendering:
![alt tag](https://github.com/zxm5010/Project4-Rasterizer/blob/master/renders/points.jpg)

Simple Displacement:
![alt tag](https://github.com/zxm5010/Project4-Rasterizer/blob/master/simple_displacement.jpg)

