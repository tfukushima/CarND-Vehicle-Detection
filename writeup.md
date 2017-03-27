Project 5 - Vehicle Detection and Tracking
==========================================

This is the copy and the excerpts from the Jupyter notebook. Please see the [notebook](Project%205%20-%20Vehicle%20Detection%20and%20Tracking.ipynb) for more details.


## Histogram of Oriented Gradients (HOG)

### Explain how (and identify where in your code) you extracted HOG features from the training images. Explain how you settled on your final choice of HOG parameters.

I ended up with having the LUV color space and the all HOG channel as the
extracted features. It takes some time to extract features based on them but it
gave the solid data. The extracting the features dominates the creating the
classifier and it takes more time than training the SVC but once it is trained,
it gives the fast predictions with the relatively high accuracy.

### Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I used the following parameters:

```
color_space = 'LUV' # Can be RGB, HSV, LUV, HLS, YUV, YCrCb
orient = 9  # HOG orientations
pix_per_cell = 8  # HOG pixels per cell
cell_per_block = 2  # HOG cells per block
spatial_size = (32, 32)  # Spatial binning dimensions
hist_bins = 32  # Number of histogram bins
hog_channel = "ALL" # Can be 0, 1, 2, or "ALL"
spatial_feat = True # Spatial features on or off
hist_feat = True # Histogram features on or off
hog_feat = True # HOG features on or off
y_start_stop = [400, None] # Min and max in y to search in slide_window()
```

Basically I used all of the features including spatial features, histogram
features and hog features.

## Sliding Window Search

### Describe how (and identify where in your code) you implemented a sliding window search. How did you decide what scales to search and how much to overlap windows?

`pix_per_cell = 8` and in `find_cars`, `cells_per_step = 2`. So each window is
overlapping  75%75% . The window size is  8x8=648x8=64  pixels. I used
`[0.75, 1, 1.5, 2, 3]` as searching scales in `detect_car` considering this
window size. The bigger scales are selected since the closer cars shall be
detected better comparing to the cars in the distances for the safeties of the
drivers.

### Show some examples of test images to demonstrate how your pipeline is working. How did you optimize the performance of your classifier?

Searching car in the different scales can be parallelized. The image where the car are detected is shared but it is not modified and only the points represesnt areas that can have car images are returned to the caller of the car detecting function. So I employed `multiprocessing` module of Python and made each car finging is executed in a newly spawned process.  Then I wrote a wrapper that put the retrieved points into the shared thread-safe list.

This worked well on the image provided by Udacity but the processes are crashed when I tried it on my laptop that runs Mac OSX.


## Video Implementation

### Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

[The link to the video](output_images/detected_car.mp4).


### Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

## Discussion

### Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?





