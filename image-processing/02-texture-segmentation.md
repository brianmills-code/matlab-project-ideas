# Image Processing Project 2: Texture Segmentation and Region Measurement

## Goal

Segment an image containing regions with different textures, measure the resulting regions, and evaluate segmentation quality against a manually labeled reference.

## Input and learning outcomes

Use a texture image from the [USC-SIPI Image Database](https://sipi.usc.edu/database/) or another public, license-compatible source. Record the exact image name, source, and any preprocessing. The project practices local statistics, filter banks, feature images, clustering, morphology, connected-component labeling, and quantitative evaluation.

## Requirements

Convert the image to grayscale and construct a feature vector for each pixel or non-overlapping patch. Suitable features include local mean, local variance, gradient magnitude, and responses from oriented filters. Normalize feature columns, cluster them into a documented number of regions, and reshape the labels into an image.

Clean small isolated regions with morphological opening or area filtering. Measure each retained region's area, centroid, bounding box, and mean intensity. Display the original image, selected feature maps, raw segmentation, and cleaned segmentation in a figure saved as `texture_segmentation.png`.

If a manual reference mask is available, calculate intersection-over-union, Dice score, pixel accuracy, and a confusion matrix after matching label identities.

## Suggested workflow

```matlab
I = im2gray(imread('texture_image.tif'));
I = im2double(I);

windowSize = 9; % Use an odd window size and document the choice.
localMean = imboxfilt(I, windowSize);
localMeanSq = imboxfilt(I.^2, 9);
localVariance = max(localMeanSq - localMean.^2, 0);
[Gmag, ~] = imgradient(I);

features = [localMean(:), localVariance(:), Gmag(:)];
features = normalize(features);
labels = kmeans(features, 3, 'Replicates', 5);
segmented = reshape(labels, size(I));
```

If Statistics and Machine Learning Toolbox is unavailable, implement a simple iterative nearest-centroid clustering routine and document it.

## Validation checklist

Inspect whether the selected features separate regions for understandable reasons. Test more than one cluster count and report how the result changes. Ensure morphology does not erase small but meaningful regions, and use a fixed random seed when clustering so results are reproducible.

## Extensions

Compare handcrafted features with a Gabor filter bank, use superpixels before clustering, add region adjacency analysis, or evaluate robustness under brightness changes and additive noise.

## References

1. [USC-SIPI Image Database](https://sipi.usc.edu/database/)
2. [MathWorks: Edge Detection](https://www.mathworks.com/discovery/edge-detection.html)
