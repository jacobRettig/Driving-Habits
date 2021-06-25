## Lane Detection Study

The lane recognition algorithm is largely divided into a method using Hough transform and a method using inverse projection. 

I will study about Hough transform method using Canny edge detection algorithm. 

After detecting an edge on an image using Canny edge detection algorithm, the part representing lane among the edges is detected by using Hough transform.

- Edges will be detected by Canny edge detection algorithm
- Lanes will be detected by Hough transform method based on the above result.

### Steps

#### 1. Preprocessing

Before applying the algorithms, we will have to reduce noises in the images to enhance the accuracy.

There are 3 ways to do this,

1. median filtering

   - the process of setting a region within an image, and making all pixel values the same. (median)

2. sharpening

   - Make a scrambled image vivid.

3. gaussian filtering

   - **We will perform filtering using a Gaussian normal distribution with a mean of 0.**

   - The farther away from 2D the less weight
   - ![2021-06-10 16_59_52-[영상처리] 차선인식 알고리즘 (1) - Canny edge detection ① 전처리 (noise reduction) _ 네이버 블로그](C:\Users\parkg\Desktop\Screenshots\2021-06-10 16_59_52-[영상처리] 차선인식 알고리즘 (1) - Canny edge detection ① 전처리 (noise reduction) _ 네이버 블로그.png)
   - ![2021-06-10 16_59_46-[영상처리] 차선인식 알고리즘 (1) - Canny edge detection ① 전처리 (noise reduction) _ 네이버 블로그](C:\Users\parkg\Desktop\Screenshots\2021-06-10 16_59_46-[영상처리] 차선인식 알고리즘 (1) - Canny edge detection ① 전처리 (noise reduction) _ 네이버 블로그.png)

#### 2. Applying Canny Edge Detection Algorithm

This step can be divided into 2 parts,

1. Finding the edge of an object or area in the image
2. Finding the road lane edges among the edges

**Canny Edge Detection Steps**

1. Smoothing using Gaussian Filtering
2. Finding gradients using sobel mask
   - where the pixel values change dramatically
     - because, road = grey, lane = white
     - ![2021-06-10 17_18_13-lane detection - Google 검색](C:\Users\parkg\Desktop\Screenshots\2021-06-10 17_18_13-lane detection - Google 검색.png)
3. Non-maximum suppression
   - based on the detected edges from the above step,
   - compare adjacent pixels of those pixels, 
   - remove non-maximum value pixels.

4. Double Thresholding
   - classifying weak edge and strong edge based on two thresholds
5. Edge tracking
   - connect the disconnected edges.

Basically, edge detection is a method of finding a boundary between two regions with different intensities.

##### Mathematical Explanation

![2021-06-10 17_34_07-2021-06-10 17_32_30-[영상처리] 차선인식 알고리즘 (2) - Canny edge detection ② mask를 이용한 edge](C:\Users\parkg\Desktop\Screenshots\2021-06-10 17_34_07-2021-06-10 17_32_30-[영상처리] 차선인식 알고리즘 (2) - Canny edge detection ② mask를 이용한 edge.png)

If we look at the above image,  we can see that the colors are dark, bright, dark.

if we draw a graph according to the brightness change on the x-axis, a waveform is generated.

If the waveform is first differentiated, a waveform indicating the intensity slope change is created.

If the result of the above waveform is second differentiated, we can check the position of the light and dark parts of the pixel representing the edge.

The part that pops out in the graph is the edge.  

##### How to apply it in the implementation?

In programming, we can replace it with a filtering operation using mask which allow us to extract the boundaries.

- **Sobel Mask**
  - Detects edges in all directions -> but reacts more sensitively to edges in diagonal directions
  - Noise-resistant because pixel values are averaged.
  - ![2021-06-10 17_43_14-[영상처리] 차선인식 알고리즘 (2) - Canny edge detection ② mask를 이용한 edge 검출 _ 네이버 블로그](C:\Users\parkg\Desktop\Screenshots\2021-06-10 17_43_14-[영상처리] 차선인식 알고리즘 (2) - Canny edge detection ② mask를 이용한 edge 검출 _ 네이버 블로그.png)
  - The left mask is for vertical direction, right one is for horizontal direction.
  - In canny detection, the edge is obtained by detecting the edge by calculating the magnitude of the two values obtained by convolving these two sobel masks with the image.

Through this mask, pixels except edges (or considered as edges) will have 0 values.

**Good notes to understand 2D convolution**

- **http://www.songho.ca/dsp/convolution/convolution2d_example.html**
- **http://www.songho.ca/dsp/convolution/convolution.html**

After this process, we will use non-maximum suppression

- To make these edges sharper or thinner. 

#### 3. Non-Maximum Suppression

Non-maximum suppression refers to thinning the edges obtained through image processing.

Edges found using gaussian filtering and sobel mask are blurred, in other words, crushed/scrambled edges. So we have to use this method to find a clearer line. 







### Current Programming Process

![2021-06-11 13_01_42-LaneDetection - Jupyter Notebook](C:\Users\parkg\Desktop\Screenshots\2021-06-11 13_01_42-LaneDetection - Jupyter Notebook.png)

![2021-06-11 13_01_46-LaneDetection - Jupyter Notebook](C:\Users\parkg\Desktop\Screenshots\2021-06-11 13_01_46-LaneDetection - Jupyter Notebook.png)



![2021-06-11 13_01_50-LaneDetection - Jupyter Notebook](C:\Users\parkg\Desktop\Screenshots\2021-06-11 13_01_50-LaneDetection - Jupyter Notebook.png)

![2021-06-11 13_01_53-LaneDetection - Jupyter Notebook](C:\Users\parkg\Desktop\Screenshots\2021-06-11 13_01_53-LaneDetection - Jupyter Notebook.png)





## LaneNet

```py
pip install tensorflow-estimator==1.15.0
conda install tensorflow
```

















