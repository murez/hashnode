## CS272 Computer Vision Project Presentation

## beginning

![](https://i.imgur.com/U8NWXUh.png)



In this project, we propose using semantic information to create synthesize novel views  with semantic reasonability and 3D consistency from a single image. 

## Background and Task 
![](https://i.imgur.com/tEpoyuP.png)

Imagine that you woke up in an unfamiliar house, like the picture, how do you imagine the scene around you?


![](https://i.imgur.com/8LkzyfJ.png)


For this task, View synthesis is the most considered problem in computer vision. It is desirable to endow the model with the ability to understand the scene comprehensively to synthesize a novel view. View synthesis is challenging given only one image, especially when the view change is large.

## Related Work
![](https://i.imgur.com/rrtFIjq.png)

Here are some related works. SynSin introduces a novel differentiable point cloud renderer that is used to transform a latent 3D point cloud of features into the target view. And PixelSynth uses deep autoregressive modeling to facilitate high-quality extrapolations in conjunction with 3D modeling to ensure consistency.

![](https://i.imgur.com/GOb4pv1.png)



But, most of the previous works about View Synthesis for Single Image rely on monocular depth estimation to predict warps. To handle disocclusions, most of these methods rely on adversarial losses, inspired by generative adversarial networks (GANs) to perform inpainting in the missing regions. However, the quality of these approaches quickly degrades for larger viewpoint changes because of the lack of information.

![](https://i.imgur.com/lrnra87.png)

Moreover, in conditional image synthesis, semantic segmentation mask is often utilized as a condition to restrain the synthesis from being reasonable and therefore generates realistic images.

![](https://i.imgur.com/morPyC5.png)

To understand the whole scene, in this paper, we present an approach to synthesize the novel view utilizing semantic information.

## Our Approach
Here is Our Approach, 
![](https://i.imgur.com/b0oLLtD.png)

We focus on estimating the depth map ds and semantic segmentation Ms from the source view image via two simple encoder-decoder networks based on ResNet18. With estimated depth and camera position, we warp the source image and source semantic segmentation to the target view. Precise and reasonable semantics can effectively guide image synthesis. We also adopt an encoder-decoder network to inpainting warped semantic segmentation.




![](https://i.imgur.com/ybagMPR.png)

We then infer the Semantic Vectors Generation(SVG) taking the inpainting semantic segmentation as input to synthesis coarse novel view structure image Ct, which can guide target view room structure. 

The semantic intermediate features from Semantic Vectors Generation(SVG) are fed into the corresponding layer of Semantic Render Generator(SRG) by parameterization. The warped image is resized to the same size as corresponding semantic intermediate features to provide more image details.

![](https://i.imgur.com/mfKMawt.png)


With inpainting segmentaton Mt, the synthesised invisible region maintain semantic reasonsbility but not in stylization. We encoder the source image Isas a style information code as the initial feature of Semantic Render Generator(SRG) to address this issue. 

![](https://i.imgur.com/BWaIWCj.png)

Synthesized images can continually produce novel view images with depth estimation and semantic estimation networks.


## Dataset
![](https://i.imgur.com/kHYfjzc.png)

As existing indoor scene datasets lack accurate depth and semantic information, severely limiting indoor scene understanding development, 
![](https://i.imgur.com/vLppGNd.png)

we synthesize a large, well-labeled indoor, consisting of 38751 images and 961 trajectories with accurate depth and semantic information and the camera intrinsics and camera extrinsic for each frame. 



## GAP3 and GAP5
![](https://i.imgur.com/Y4vsc5d.png)

To carry out the experiment, two tasks, GAP3 and GAP5, are created based on the Dataset. In the GAP3 task, we move number i frame to number i plus 3 in the trajectory sequence, and in the GAP5 task, we transfer ith to number i plus 5 frame. 
Inverse GAP3 and Inverse GAP5 are configured to thoroughly validate our model.

## Experiment

### Evaluation Metrics.
![](https://i.imgur.com/Ppm5Qk8.png)



The major evaluation procedure for our work is determining how similar the synthesized image is to the ground-truth image. As a result, we use metrics such as PSNR, perceptual similarity, and FID to generate a more robust indication of the quality of our work.

### Baselines

We evaluate our model with the following SOTA methods in the space of synthesizing unseen parts of rooms: SynSin and PixelSynth.


## Evaluation Quailites
![](https://i.imgur.com/tlur98u.jpg)

The table below reports the results of all methods on GAP3 and GAP5 tasks. Our methods achieve better results in PSNR and perc sim.

In comparison to those two approaches, our results provide a clear and rather accurate image of the target direction due to the depth estimate module.

## Ablation Studies Semantic Feature Generation Loss

![](https://i.imgur.com/W33esbw.png)

in our pipeline, we take the source view image and warped image as input. Therefore, the generation from semantic degenerates from coarse target view image generation to target view image structure generation. We select different coefficients for ablation studies to choose the best loss coefficient, when lambda s equals to 5 or 10 , we can have the best reconstruction of the layout.



## Ablation Studies Camera Moving Direction

![](https://i.imgur.com/zRmZ9ky.png)

In most new view synthesis, the camera moves from source to target. The invisible area's color can be derived from the visible area. As a result, the work resembles image interpolation. Inverting the frames makes the camera move backward. Invisible regions' color information can be difficult to acquire from accessible regions, making innovative perspective synthesis more difficult. In this experiment, we reverse the GAP3 and GAP5 tasks. As seen in the table, our model shows the inverse direction clearly and accurately.

## Ablation Studies semantic reasonability and 3D consistency

![](https://i.imgur.com/bcsSnrZ.jpg)


And from this comparison, our model gives a more reasonable iamge of the novel view, seeing that the prediction of the light position, TV angle and the ceiling area are more accurate and consistent thanks to the semantic infomation.

In conclusion, we propose a semantic-guided novel view synthesis pipeline to generate a photo-realistic novel view image with large view change and maintain semantic reasonability and 3D consistency. Our approach makes it possible to explore invisible regions in large view change. We present a large-scale indoor dataset with accurate depth and semantic segmentation to fill the blank. We conduct an extensive experimental result to validate our model design and show good performance in generating photo-realistic novel view images.