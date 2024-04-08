# VR-AR-CG-Network Telemetery 

## (1) AR traffic collection
In this experiment, we consider the AR devices (e.g. AR glasses) and AR server as mentioned in **ITU-T Rec. Q.4066 (09/2020) Testing procedures of augmented reality applications** (Fig.7-1). 
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/dda5bf05-8567-4549-81ac-6a493fdcff9e)

### (1-1) Experiment-1 [AR traffic generation (without glasses)]
The experiment setup is shown in the following figure:

 ![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F18723381%2F9b91e482bc29c99457ec12b41790d4a2%2FAR%20Senario(60).png?generation=1708380734927241&alt=media)

we need to generate and stream the video from the scenes (as if user is looking at the environment) based on the specific frame rate and resolution:

![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F18723381%2F7a3bd66e12f7e062465ab4c62aa62347%2FStreams.png?generation=1708380591528417&alt=media)

The seven environment frames, publish by Microsoft in https://www.microsoft.com/en-us/research/project/rgb-d-dataset-7-scenes/, are used to make it close to user experience!
Each sequence (seq-XX.zip) consists of 500-1000 frames. Each frame consists of three files:

    **Color**: frame-XXXXXX.color.png (RGB, 24-bit, PNG)

    **Depth**: frame-XXXXXX.depth.png (depth in millimeters, 16-bit, PNG, invalid depth is set to 65535).

    **Pose**: frame-XXXXXX.pose.txt (camera-to-world, 4×4 matrix in homogeneous coordinates).

The OS of the systems are Linux Ubuntu 22.04 LTS and the following instructions will be executed to install requirements and do the experiment step by step! 

**(1-1-1) Install FFmpeg to generate video with specific frame rate and resolution with frames!**  

    #sudo apt-get update && sudo apt-get dist-upgrade
  
    #sudo apt-get install ffmpeg

**(1-1-2) Generate Video using sequential frames**

    #ffmpeg -r [frame rate] -f image2 -s [resolution] -i [sequence of png files] -vcodec libx264 -crf 25 -pix_fmt yuv420p [video name in mp4]
    
    -r sets the frame rate to 30 fps.
-f image2 tells FFmpeg to use its image2 demuxer, which is designed for image sequences.
-s 1920x1080 sets the resolution of the output video. You should change this to match the resolution of your images.
img%03d.png is a pattern for input file names (e.g., img001.png, img002.png, etc.).
-vcodec libx264 specifies that the output video should use the H.264 codec.
-crf 25 sets the Constant Rate Factor to 25, which is a good balance between quality and file size.
-pix_fmt yuv420p sets the pixel format, which is widely compatible with various devices and services.
video.mp4 is the name of the output video file.

    
    video1080_60.mp4
frame-%06d.color.png
1920x1080

## (2) CG traffic collection
