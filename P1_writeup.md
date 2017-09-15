# **Finding Lane Lines on the Road** 

## By David Dietz

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Description of my pipeline.

My pipeline starts (after the provided helper functions) with a new object definition and constructor for the line detection paramaters. Since there are many paramaters for detecting lines I decided to use an object to help keep track of them. I then defined all of the attributes (expect vertices which is only initialized), along with source and desination folders within the local operating system where the images will be pull from and output to. A new object (params) is created and set with the newly defined attributes and additional functions are defined in order break up the pipeline and assist with resuseability. 

The job of the pipeline is to take an one or more images from the source folder, detect the lines, overlay the images with a semi-transparent line, and saving the result to a desination folder. I broke out the reading in the images and applying the line definition so I could read in an array of images and loop over them applying lane detection to each. Later when a video reads one image at a time I am able to call the lane detection function directly.
Here is a breakdown of the proccess of applying lane dection and overlaying the original image.

####Read in the original image####

![Original Image](/writeup_images/whiteCarLaneSwitch.jpg )

####Apply Lane Detection####
* Make copy of original image - lets call it copyImage
* Apply greyscale to copyImage

    ![Greyscale Applied](/writeup_images/greyscale.png )
* Apply gaussian_blur and canny transform to copyImage

    ![Canny Applied](/writeup_images/canny.png )

* Mask copyImage to include only area of interest

    ![Mask Applied](/writeup_images/masked.png )

* Apply hough transform to copyImage and draw lines

    ![Hough Lines Applied](/writeup_images/houghLines.png )
    
####Overlay copyImage (lines only) with the original image####

![Overlay Original](/writeup_images/overlay.png )

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by finding the the slope (of every line) and extracting the x,y points (adding them to an array) by positive or negative slope value. I then calculated the average slope and y intercept of each array (using the numpy polyfit function). Using those values along with the minimum, maximum and image frame I was able to draw a continuious line (for both left and right lines) across all lines that matched my criteria. Note that the image had already been masked by this point so I was only analyzing lines in my area of interest.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when any number of variables are intoduced to the image. These could include:
* Other vehicles entering area of interest
* Shadows
* Horizontal line (crosswalks, road markings, etc.)
* Nighttime elements (light, contrast)
* Curvy roads
* Camera shifting/shaking(blurring image)
* Debris on camera when picture is taken
* Steep incline or decline (i.e. changing perspective leading to area of interest change

Another shortcoming could be memory/processing useage. It is not optimized and would be slow to process if introduced in a live setting.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to do more preprocessing of the image. This could help account for changing variables and make adjuestments if needed.

Another potential improvement could be to write in fail safes and tests. Right now the pipeline has one specific "happy path". I could build in some checks that would allow failures/errors/edge-cases to behave differently without crashing the application. Also I would feel more confident in the pipeline if I could write unit tests around it.
