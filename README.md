# VR-AR-CG-Network Telemetery 

## (1) AR traffic collection
To simulate the AR application to collect the network traffic dataset, we consider application testing model used in **ITU-T Rec. Q.4066 (09/2020) Testing procedures of augmented reality applications** (Fig.7-1). 
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/dda5bf05-8567-4549-81ac-6a493fdcff9e)

### (1-1) Experiment 1 
### ***[AR traffic collection (without glasses)]***
We have three componenets for simulation:

  ***-XR (AR or VR) glasses:*** XR (AR or VR) glasses is simulated with a computer streaming the scenes frames in specific ****resolution**** and ****frame rate**** (accordance with these features in the off-the-shelf XR glasses).

  ***-Network:*** The server and XR glasses are connected using Wi-Fi-5.0

  ***-Server:*** The server is a computer system receive the frames! and collected 



XR (AR or VR) glasses is simulated with a computer and it is assumed that there is an AR glasses which look at the scenes and streams the video with specific ****resolution**** and ****frame rate**** (accordance with these features in the off-the-shelf XR glasses) to the edge server!
This XR simulated glasses can receive the scene with augmented digital object! (Full offloading). The experiment setup is shown in the following figure:

 ![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F18723381%2F9b91e482bc29c99457ec12b41790d4a2%2FAR%20Senario(60).png?generation=1708380734927241&alt=media)

We need to generate and stream the video from the scenes (as if user is looking at the environment) as a XR (AR or VR) glasses based on the specific frame rate and resolution as mentioned in the following table:

![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F18723381%2F7a3bd66e12f7e062465ab4c62aa62347%2FStreams.png?generation=1708380591528417&alt=media)

The seven environment frames, publish by Microsoft in https://www.microsoft.com/en-us/research/project/rgb-d-dataset-7-scenes/, are used to make it close to user experience!
Each sequence (seq-XX.zip) consists of 500-1000 frames. Each frame consists of three files:

**Color**: frame-XXXXXX.color.png (RGB, 24-bit, PNG)

**Depth**: frame-XXXXXX.depth.png (depth in millimeters, 16-bit, PNG, invalid depth is set to 65535).

**Pose**: frame-XXXXXX.pose.txt (camera-to-world, 4×4 matrix in homogeneous coordinates).

In this experiment, we have two computer systems whose OS are **Linux ubuntu 22.04 LTS**. The computer which generates the stream as the XR (VR or AR) glasses will be called **XR system** and the computer simulated edge server is called **edge server**.
To execute the commands, the name of the simulation system will be mentioned!

**(1-1-1) Install FFmpeg [XR system]!** [https://ffmpeg.org/]  
This tool uses the set of frmaes (in png format) to generate video in specific frame rate and resolution!

    # sudo apt-get update && sudo apt-get dist-upgrade
  
    # sudo apt-get install ffmpeg

    # ffmpeg -version
**Output:**
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/2eac8996-967f-4291-bd0d-842f2f5534c2)


**(1-1-2) Generate video using sequential frames [XR system]**

    # ffmpeg -r [frame rate] -f image2 -s [resolution] -i [sequence of png files] -vcodec libx264 -crf 25 -pix_fmt yuv420p [video name in mp4]
 
 ***- [frame rate]*** -- > e.g. 30, 60, 90, 120 (fps)
 
 ***- [resolution]*** --> e.g. 1920x1080 

 ***- [sequence of png files]*** --> e.g. img%03d.png  (for the files with img001.png, img002.png, ... , img999.png)

 ***- [video name in mp4]*** --> e.g. my_video_1920_1080.mp4

 ***- libx264*** --> -vcodec libx264 is to set the encoding

**For example:**
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/14b47fb2-f9df-4383-a5b0-27fe29d9a45d)
**Output:**
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/834385dc-0b05-4c88-81ed-b97b81f7f4a3)

**(1-1-3) Install gst-launch for video streaming [XR system]** 
[https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c]

    # sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio

**(1-1-4) Stream the video with specific resolution, frame rate, encoding and bitrate [XR system]**

    # gst-launch-1.0 -v filesrc location=./video1080_30.mp4 ! decodebin ! videoconvert ! videoscale ! video/x-raw,width=1920,height=1080 ! videorate ! video/x-raw,framerate=60/1 ! x264enc tune=zerolatency bitrate=5000 ! rtph264pay config-interval=1 pt=96 ! udpsink host=[IP address] port=[Port#]
    

***- [location=./video1080_30.mp4]*** --> location of the video

***- [width=1920,height=1080]***-->  resolution for streaming (This option can be neglected because it depends on the resolution of the video!)

***- [framerate=60/1]*** --> frame rate of the streaming (This option can be neglected because it depends on the frame rate of the video!)

***- [x264enc]*** --> The encoding which is H.264

***- [bitrate=5000]*** --> It is the bitrate of sampling! (More bitrate higher sampling and higher video quality!)

***- [rtph264pay]*** --> It is RTP protocol with H.264  encoding!

***- [IP address]*** --> the edge server IP address e.g. 192.168.10.2

***- [Port#]*** --> the port number e.g. 5000

**Output:**

![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/dbf7b664-52af-4d19-816d-5f155fb9058a)


## (2) CG traffic collection