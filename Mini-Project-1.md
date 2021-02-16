# Mini Project 1

Detect social distancing status in a video. 

[Input video link](https://youtu.be/H3avoeBbqGA)

[output video link](https://youtu.be/LG4kLRvcNOc)


## Question 1
**Was your social distance detector effective? Describe how the detector shows potential violations**

Answer: For each human figure, the social distance detector creates a frame around it. If the frame is green, the person is safe; if the frame is red, then the person violates social distancing according to the detector. For the input video, the detector is partially effective at locating violations for players on the court. However, it misrepresents the audience at the background as one big figure. 


## Question 2
**Is the approach effective for estimating new infections in real time? How would you implement such an approach in response to the COVID-19?**

Answer: The approach is **NOT** effective at real time, as the detector took 2.5 minutes to process a video of only 9 seconds. The resolution of the output video was incredibly low and we could barely recognize the people with red frames. Moreover, the detector was poor at examining people at the background.

The detector could be used for verfication if we notice potential violations of social distancing. For example, if there is a gathering of a small number of people in a enclosed space, we can take a short video and of the crowd and use the detector to check whether anyone potentially violates social distancing.

## Question 3
**What limitations or improvements might you include in order to improve your proposed design?**

Answer: Work on small groups of people. Locate the camera at an upper angle so that the detector could catch everyone in the screen without two figures overlapping.

