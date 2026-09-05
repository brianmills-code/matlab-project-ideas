# Image Processing Project 1: Document Scanner and Perspective Correction

## Goal

Create a MATLAB pipeline that detects the boundary of a photographed document, corrects perspective, enhances readability, and exports a clean scanned image.

## Input and learning outcomes

Use photographs taken by the student or a properly licensed document-image dataset. Do not include private personal information in a public repository. The project practices grayscale conversion, denoising, edge detection, morphology, connected components, projective transformation, and adaptive thresholding.

## Requirements

Read an RGB photograph, correct uneven illumination, and detect the largest plausible quadrilateral corresponding to the document. Use edge detection followed by morphological cleanup and connected-component analysis. Order the four corners consistently, compute a projective transform, and warp the document to a rectangular output.

Produce both a grayscale enhanced version and a binary version. Compare global thresholding with adaptive thresholding. Save a three-panel figure as `document_scanner_result.png` and write the corrected image as `scanned_document.png`.

## Suggested workflow

```matlab
rgb = imread('document_photo.jpg');
gray = im2gray(rgb);
gray = adapthisteq(gray);
edges = edge(gray, 'Canny');
clean = imclose(edges, strel('disk', 3));
clean = imfill(clean, 'holes');
components = bwconncomp(clean);
props = regionprops(components, 'Area', 'BoundingBox', 'ConvexHull');

% Select and inspect the largest plausible document component.
[~, index] = max([props.Area]);
outline = props(index).ConvexHull;
% Order four selected corner points, then use fitgeotrans and imwarp.
```

Document the assumptions used to select the document, such as area, convexity, and aspect ratio. If automatic corner detection fails, include a clearly documented manual-corner fallback for comparison.

## Validation checklist

Test the pipeline on at least five images with different lighting and rotations. Check that the output aspect ratio is reasonable, text remains readable, and the document boundary is not clipped. Compare the automated corners with manually marked corners using average corner error.

## Extensions

Add shadow removal, page de-skewing, automatic orientation detection, OCR integration, or batch processing with a quality score for each output.

## References

1. [MathWorks: Edge Detection](https://www.mathworks.com/discovery/edge-detection.html)
2. [MathWorks: Detecting a Cell Using Image Segmentation](https://www.mathworks.com/help/images/detecting-a-cell-using-image-segmentation.html)
3. [USC-SIPI Image Database](https://sipi.usc.edu/database/)
