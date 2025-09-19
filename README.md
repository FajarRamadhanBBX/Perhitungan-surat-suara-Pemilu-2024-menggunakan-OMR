# 2024 Pemilu Election Calculation Using Optical Mark Recognition
OMR (Optical Mark Recognition) is a technique for detecting the presence of marks within an image. In this case, OMR is used to determine the number of votes a candidate received on the ballots used for the 2024 Election. This project utilizes OpenCV and Python.

## 🎯 Project Overview

Manual vote counting is a time-consuming and error-prone process. This project aims to automate the tallying of votes from a specific ballot format used for counting candidate scores. The system processes an image of the ballot, identifies the marked bubbles for each candidate, and translates these marks into a final vote count.

## 🔄 Workflow

The process is broken down into two main stages:
1.  **Image Preprocessing:** Cleaning and preparing the ballot image to make it suitable for analysis. This involves correcting perspective, improving contrast, and reducing noise.
2.  **Optical Mark Recognition (OMR):** Developing a logic to detect and count the marked bubbles for each candidate's score.

### 1. Image Preprocessing
-  **Load Image in Grayscale:** The image is loaded in grayscale to simplify processing.
-  **Gaussian Blur:** A (3x3) Gaussian blur is applied to smooth the image and reduce minor noise.
-  **Contrast Enhancement (CLAHE):** Contrast Limited Adaptive Histogram Equalization (CLAHE) is used to improve the local contrast of the image, making the marks more distinct.
-  **Perspective Transform:** To correct for any angular distortion from the scanning or photographing process, a perspective transform is applied. This "flattens" the image, making the ballot appear as if it were viewed from directly above.
-  **Image Sharpening:** A sharpening kernel is applied to enhance the edges of the objects, particularly the bubbles, making them easier to detect.

### 2. OMR
-  **Binarization:** The ROI is converted into a binary (black and white) image using a threshold.
-  **Contour Detection:** OpenCV's `findContours` function is used to identify all distinct shapes, which in this case are the circular bubbles.
-  **Filtering & Sorting Contours:**
    * Contours are filtered by area to discard any noise or irrelevant shapes.
    * The detected bubbles are first sorted vertically (top-to-bottom) and then divided into three columns representing the hundreds, tens, and units digits.
-  **Vote Counting per Column:** The system iterates through the bubbles in each column from top to bottom (0 to 9). It counts the number of unmarked bubbles by analyzing the density of black pixels within each bubble's bounding box. The counting stops when a marked bubble (exceeding a pixel density threshold) is found. The number of unmarked bubbles before the first marked one gives the digit for that column.
-  **Final Tally:** The digits from the three columns are concatenated to produce the final vote count for the candidate.

## Future Improvement
Currently, the detection of boxes containing number markers is still done manually. In the future, this mechanism can be automated.
