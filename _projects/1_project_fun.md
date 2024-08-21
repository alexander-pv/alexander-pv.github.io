---
layout: page
title: Algorithmic emoji style transfer 
description: Transform an emoji into a given contour of another one
img: 
importance: 1
category: fun
related_publications: false
---

Initial source of the idea: 
[OpenCV transform image shape transformation into a given contour](https://stackoverflow.com/questions/71443071/opencv-transform-image-shape-transformation-into-a-given-contour)

The core solution is to align points from one image contour to another one. But how to do it without manual operations?
That was the main question I tried to solve. As a result, I wrote three methods to tackle the task:

1. **Contour points sampling, CPS**
   1. Get images of the equal size
   2. Extract object mask
   3. Extract object contour
   4. Sample contour for each image
   5. Calculate distance matrix & perform linear sum alignment to get the proper points pairs
   6. Split images into triangles and perform affine transformations and warping to fit triangles to the primary image.
2. **Contour areas stratification, CASv1**
   1. Get images of the equal size
   2. Extract object mask
   3. Extract object contour
   4. Extract bbox of a contour
   4. Find closest contour points to bbox vertices
   5. Split contour points based on closest vertices to areas: top, bottom, left, right. Combine sorted areas.
   6. Split images into triangles and perform affine transformations and warping to fit triangles to the primary image.
3. **Contour areas stratification v2, CASv2**
   1. Get images of the equal size
   2. Extract object mask
   3. Extract object contour
      4. Get contour of a mask
      5. Get convex hull of the previously extracted contour
      6. Create a new mask from the resulting convex hull
      7. Get final contour from the new mask
   4. Extract bbox of a contour
   4. Find closest contour points to bbox vertices
   5. Split contour points based on closest vertices to areas: top, bottom, left, right. Combine sorted areas.
   6. Split images into triangles and perform affine transformations and warping to fit triangles to the primary image.

Examples:

![img-1](/assets/img/projects/fun/img_warper/image_warper-1.jpg)

![img-2](/assets/img/projects/fun/img_warper/image_warper-2.jpg ) ![img-3](/assets/img/projects/fun/img_warper/image_warper-3.jpg ) 
![img-4](/assets/img/projects/fun/img_warper/image_warper-4.jpg ) ![img-5](/assets/img/projects/fun/img_warper/image_warper-5.jpg ) 
![img-6](/assets/img/projects/fun/img_warper/image_warper-6.jpg ) ![img-7](/assets/img/projects/fun/img_warper/image_warper-7.jpg )

[Link to the GitHub repository](https://github.com/alexander-pv/image_warper)
