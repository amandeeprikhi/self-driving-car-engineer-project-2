# **Advanced Lane Finding Project**

**I have attached the following with the project:**

* Writeup.md
* Writeup.pdf
* Project_2.ipynb(Jupyter Notebook)
* Project_2.html

**The goals / steps of this project are the following:**

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  
---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

* The camera matrix and distortion coefficients were calculated using the **cv2.findChessboardCorners()** and **cv2.calibrateCamera()** methods.

* Object_points were calculated by processing a np.array and reshaping it in order to obtain the object_points matrix.

* Image_points were calculated using the chessboard corners obtained using the **findChessboardCorners()**

Attached are the original and un-distorted images:

![Original and Undistorted Image](https://lh3.googleusercontent.com/BHCn7XSeVmeplRhiZnwBOkiqa_6m5lJdU1JPjmZ-U6uSnhMHLi4CS_KAK9y3qE160o3_ZSdBvCXHkAxCQCB4v5YzQApXxa-V7H3J-ZGaf2KV09Srz_Z1oZAONkcYGpN2xwh-Xzuwoto8Xnwfke2Cc9vbZbJVstSs2J-h9HAyJd3IxEboPojmwYq4nyV06ipJK_nmNjww7zMBjudUX5W7L-xv9lCNb8vJK86V3FRNuWDhvh7Nxkebfyj_82tIZVjSFhJgbshkMNE1XUvajZ_0_hUWVimrECoR_MiJb6D4lFokv60eve034HtnX9MdH2aZdDu3WFnWO_3eRaS5akZep4rgFqNObHBZyJzW75XFWPyGTL1EIDJK1kAUgEQTe3GiKYwVn5Q4NiSKmqO9yQyTQCmCb3DM4RnLrvq7_xys7_HbH-zot7T-rgu3V60cqGpDO_PFGE9jqT6Xbkt2F4yAvUVtvQ_KWxn3RgoIVm8qaNDgIDdcX1eBM8LcWVS4EMAH8fEsCYOa4GlQPS8LWB4KMs3EQDddDfffnis2J1groyKsKgOCTNHq_xPTp8Y4hVpyHjQtHxCV6lvis_QIQJdxXTdSAA8jtetSQgCobN3f3v3AG-0ajQupYRs300SlktYs11_QAgPKyiCUOOG8WJNg4OpLa4m8kXc=w802-h236-no)

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![Original and Undistorted Image](https://lh3.googleusercontent.com/BHCn7XSeVmeplRhiZnwBOkiqa_6m5lJdU1JPjmZ-U6uSnhMHLi4CS_KAK9y3qE160o3_ZSdBvCXHkAxCQCB4v5YzQApXxa-V7H3J-ZGaf2KV09Srz_Z1oZAONkcYGpN2xwh-Xzuwoto8Xnwfke2Cc9vbZbJVstSs2J-h9HAyJd3IxEboPojmwYq4nyV06ipJK_nmNjww7zMBjudUX5W7L-xv9lCNb8vJK86V3FRNuWDhvh7Nxkebfyj_82tIZVjSFhJgbshkMNE1XUvajZ_0_hUWVimrECoR_MiJb6D4lFokv60eve034HtnX9MdH2aZdDu3WFnWO_3eRaS5akZep4rgFqNObHBZyJzW75XFWPyGTL1EIDJK1kAUgEQTe3GiKYwVn5Q4NiSKmqO9yQyTQCmCb3DM4RnLrvq7_xys7_HbH-zot7T-rgu3V60cqGpDO_PFGE9jqT6Xbkt2F4yAvUVtvQ_KWxn3RgoIVm8qaNDgIDdcX1eBM8LcWVS4EMAH8fEsCYOa4GlQPS8LWB4KMs3EQDddDfffnis2J1groyKsKgOCTNHq_xPTp8Y4hVpyHjQtHxCV6lvis_QIQJdxXTdSAA8jtetSQgCobN3f3v3AG-0ajQupYRs300SlktYs11_QAgPKyiCUOOG8WJNg4OpLa4m8kXc=w802-h236-no)

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image.  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![Original image vs Color transformed image](https://lh3.googleusercontent.com/YQCFvywZA0cu1HuuXljmKTcvE7pjYfy1BBG_xQtQRiw1SnSVvCwOgR0G8PGm6_n2mOrhT-Yq1l4cbVZlr4KpJebo-XfwY4XweuevlKYpfAzHotS29apV1nijqvybSyX3fCPDKOVen_LOnpf8lWAyTl9y6ros6TuC0lmRWFErGp83gxkwy2gdBtWbANAkAhgQoytG51WAO4SAbdFD7vNM5xqkp6urnFilPs4LbXdT3-UhnOZDH0kSCt6AIebacaoHhSD0WTXGy8kLFXmUdBJEywLncX87dL_-m2ogBHWpEKmF6vSbgXPc2D9yk_kRE0HHRiuVJH19yuzQ98Ya_Rk-2vr9CRFQxW1B6quBsh0Ef8RnN2-ZSDBAkAM96cTWzoIzvj3hrpwk8yy01FoJ396jnIN4-HBYUG9tv1If_DLx9UHYCaKnvY17Df6QPGKgGcJMjZAwjs-KoGxc6QwNt02ZlrS5Lydr2CE7pyKOb2BLu5ne47YvDVL8FcA_LFLj8Vz_lgp1y9QPJFYWOvQMf2eXUg2KRmu3TEqV6ePXgw94D2--NoslLdqBGYtYhYSeopQJrZPoQO12dvKDhmXdtRgnkR0NRtej-Mnj_ltDZOTohh24Op3Y2ZDCvwwoA6Y26bL_BYUJnK87sMJuFz98bl0mIX1L7_iP9I8=w802-h236-no)

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The notebook contains the code in order to perform the Perspective transform on an image.
* The method **perspective_transform()** performs the required steps in order to do a obtain a "bird-eye" view.
* Method **cv2.getPerspectiveTransform()** is used in order to obtain the transformation matrix.
* Method **cv2.warpPerspective()** is used in order to obtain the "bird-eye" view of the road image.

```python
src = np.float32([[190, 700], [1110, 700], [720, 470], [570, 470]])
bottom_left = src[0][0]+100, src[0][1]
bottom_right = src[1][0]-200, src[1][1]
top_left = src[3][0]-250, 1
top_right = src[2][0]+200, 1
dst = np.float32([bottom_left, bottom_right, top_right, top_left])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 190, 700      | 290, 700      | 
| 1110, 700     | 910, 700      |
| 720, 470      | 920, 1        |
| 570, 470      | 320, 1        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![Orginal Image vs Warped Image](https://lh3.googleusercontent.com/2mUAf3_PjWrN3sxVowhdYraIZ1S0k3Yc1nkWJDTJ6lTpSWk1KZTpbYJI3H51O7ht_SrtLyVfe1s3UkeWv1UB7oI5fT6dMNzX1jrXE9qJnF7GaOjlw7FqLeRGdQ0LTR6gba6J3z3MZ5mUZJcRDO5MyMgd66mSG9TtTA1Um-1_KGlNx58ryg59JOuo7-dIWpzSNL1vsDpOKqUlQh-F1fgQlENYgQEvY-zxrN7K86qMIATeppA1Gn7K-iTQJuYynF13qMPwGXy0GO5doMxsLRMyYr449O1lxRn0NRQ1si76AlihPUa0H4kPXGW7Rikba9XEex2qI5Xv4smQVASDQgeJsBnFulz0XZ08IvH2XhLjBoZWky8X_xi7XXhXnrxszzxyBwtmOCtlo1P6Q-r4xXqPhbtCTb-zFHsf0Kr5nVKUdhNW8hFfbFK47ph4Ziu2Hetu5CKcOPZjfxcpIGfFNr_dT_g1IY_kx6Ex3im82r0XOxAyASZh37WtkNY0Z_MP6pYE6XfsaY2DOE4jCh4lshwFwsC_ltP9sF-Igt50oha-OLKfYt-oyQNPKwhgws15groa_KTtAQjsUZIuqWntQNFobwfnkVKL8FxFwtuiFMpkGaRAY1f1aW_TtjBy01oRV30Kp-eDyJ-amw-9mS1TYCe2QgwbqItRSzM=w802-h236-no)

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I've referenced to the lessons provided by Udacity in order to compute the lane line pixels in the warped and color transformed images.
* First step was to plot a histogram of the image.
* Then, split the histogram into 2 so that left and right lanes can be identified by the left and right peaks of the histogram.
* A sliding window mechanism was used in order to detect the non-zero pixels in and image along the histogram peaks.
* The lane line pixels were stored in an array and their positions were fit with a polynomial.

![Original Image vs Sliding window](https://lh3.googleusercontent.com/HzzNd_v9H9e8ucd-uaq2-Q0MAJtqgKpdo12kdFCI3I4K4qxUj5llpBqHj3o5_GcH1mk8XSt37hGlS8WEirnSO0OWnzFqdfWKYEVCKaNXkvHPeO8qjrXE1r9_xas1zXoG0CcpUMMHdu3LL0xANRCU5h5BAEx1ifgYwOc0hqlNIzoZtRQO2XRPqC1i1m1W4Ln9E9mu3zdPPy5mAch2Bijk05ZFdjxzUZmoaSkSJ-70ToBP9AtRDP3DBFDLRT-9YtnIwZS6Hwl2Vg8RR19eIltDO7vrTUgE7kb1DPsHFif1nc04t6n0DCnOszGRD8iXydChZxWMuF_Ixc4Y7_Zwskuyd4214gHr9Jx9gLrS73Ddze8OvBvg-2iK_Ohm5er-HbZ1usy_GDR-d83yyEcdiphosjcCBaWurGlvPqQf4vqOFtEy2QvK1jB35SkHigmDH_g-JLSifZLwuYAFoKWZj1d_Zk14IZTFQjMTZ3i4U8MrHNQ0gd_IreotLUcGc_OOzCI-AV-wqfSBTtD8nKLEbe8rJ3Y6grxOkBBdlc43Pi9zkpW7Hcmdsu9VtBIHpPb4paPqsPNs8Z6ZsG982ETa4gR6ZNHXf6wqEh4xRCroFVVgwNA3X5S0yX-iHrvGAGMIZCJyLJiMIyfOR1KacIfjRfep03gGuFdBV24=w802-h236-no)

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

* First the lane width were determined by using the US standard line for lane width.
* ```
  left_curverad = ((1 + (2*left_fit_cr[0]*y_eval*ym_per_pix + left_fit_cr[1])**2)**1.5) / np.absolute(2*left_fit_cr[0])
  right_curverad = ((1 + (2*right_fit_cr[0]*y_eval*ym_per_pix + right_fit_cr[1])**2)**1.5) / np.absolute(2*right_fit_cr[0])
  ```
* The above methods were used in order to determine the radius of curvature of the lane. 
#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The image was transposed over the initial image in order to gain the images with lane lines marked and radius of curvature for the lane lines marked.

![Original Image vs Lane plotted](https://lh3.googleusercontent.com/bqfWHJo5kEP5H75RM_6tkf--CO4cDlQeShPYzNoYoXVS98hkvOIl6MLu_U9vqNtJXntwJX5oCfs6c3RTkqrWS3rM1IMsskrT_NJrUisI8vIHiqgQi-kodMO9fWUzXCMFQrV5EBUi8HABEC0DFjZYkzrYkboWwwAyqbVce8lUsK7-ve0ce5aHQHmy9QWn8eHfln4LAbhi2ctbjO69z6gQ44EmRMT8wErlT3anuXUYkLXEu7NzJZo98M5Tb0_e9Dc6uwFn8ZYJARDfsGIGfQ5Aj1DuSV8bVmI9KUdimEFZVUCJCUnLcgpNGhhd5Gf4LZNLDe7f3mNfFilSP0QctM-xsIOLNMnOrjK_vfWJcOn2uknYGKH2uLpyL9roXeLNpdaPlg2wMszJYnUF9nlWfUcrP-6ZXT3PuAHHs4UHRxqQVZPJI8V_jxESnED_NQY6XO_ty5Y05z_pg-3qGrmdy5HetUTGqQiT--6XtBW_1z6j-TPZ75HcN7ez0PcPDa3E4cIYWUFpteP0r3riO3lnzezvQU3Cdextd1waUwxKepI84Y_uTinqjc2IEuBwfr17QJrjk19uHAjeieLnHMF7h5F2ewfLZVVKUT68OE6OK4xwDfhhHAMn7BB2otS6HXc1D7DxeVmTTW8VS7LWhS2uC_BYdT2tsNw4Yn4=w802-h236-no)

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](https://photos.app.goo.gl/RReDAx4KSiQCPvMe7)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?


I've used the techniques that were taught to me during the lessons provided in the course.

**Shortcomings:**

* The methodology fails when there is a sharp change in the lighting conditions(harder challenge video).
* The methodology fails when there is a difference in the color gradient of the lane(challenge video).