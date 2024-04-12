# VR-AR-CG-Network Telemetery 

## (1) AR traffic collection
To simulate the AR application to collect the network traffic dataset, we consider application several experiments which  have been done with AR devices and without AR devices. 




### (1-1) Experiment1 - (without VR/AR glasses)

In this dataset, we wanted to collect the network traffic of the Augmented Reality (AR) use case in which a user is equipped with AR glasses and moving in the scene. The frames related to the scenes are sent to the edge server for rendering (UL) and AR glasses receive the rendered video (real+digital objects) from the edge server (DL). 

#### (1-1-1) Methodology

Two computers are connected via an access point, as illustrated in Figure 1. The network traffic collected at the edge server is referred to as Uplink traffic. Subsequently, video streaming, characterized by a specific resolution and frame rate with constant encoding bitrate (20-35 Mbps), is generated and designated as Downlink traffic, as depicted in Figure 2. 


<div align="center">
  <img src="AR_Senario.png">
</div>
<p align="center">
<sub>Fig.(1). Topology of the AR Network Traffic</sub>
</p>

<div align="center">
  <img src="Streaming_Features.png">
</div>
<p align="center">
<sub>Fig(2). Streams Resolution & Frame Rate</sub>
</p>

#### (1-1-2) Content

Three different types of files are available for those working on AR network traffic research.

(1) The video of seven scenes using the Microsoft frames datasets https://www.microsoft.com/en-us/research/project/rgb-d-dataset-7-scenes/ are generated in specific ***frame rate*** and ***resolution*** are available in [Here](https://kaggle.com/datasets/a906acd0ce4c8ee03048bf10c06573547ddca5a5c775ba592306bd04038f3a56) with the name of `scenes.tar.xz'.

(2) The videos are streamed on the Wireless network with the topology as shown in the paper and network traffic is collected with Tshark in PCAP format. This dataset is collected in 3459 sec (~57min and 39sec). The experiment is repeated two times, so the collected PCAP files are available in PCAP1 and PCAP2 folders. These folders are compressed (PCAP1.tar.xz and PCAP2.tar.xz). [Here!](https://kaggle.com/datasets/a906acd0ce4c8ee03048bf10c06573547ddca5a5c775ba592306bd04038f3a56)

(3) The CSV datasets, created through statistical distribution with parameters outlined in the paper, are organized into three files:
- ***`AR.csv'***: Contains 5000 samples, encompassing both Uplink (UL) and Downlink (DL) data.
- ***`DL.csv'***: Comprises 2000 samples, specifically representing Downlink data.
- ***`UL.csv'***: Includes 3000 samples of Uplink data.




testing model used in **ITU-T Rec. Q.4066 (09/2020) Testing procedures of augmented reality applications** (Fig.7-1). 
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/dda5bf05-8567-4549-81ac-6a493fdcff9e)

To simulate the AR application traffic, There are four componenets:

  - ***XR (AR or VR) glasses***: it is simulated with a computer streaming the scenes frames in specific ****resolution**** and ****frame rate**** (accordance with these features in the off-the-shelf XR glasses).

  - ***Network***: The server and XR glasses are connected using Wi-Fi-5.0

  - ***Server***: The server is a computer system receive the frames! and collected 
  
  - ***Network Traffic Collector Agent***: The agent collect the network traffic in pcap format (tshark)



### (1-1) Experiment1 
### ***[AR traffic collection (without glasses)]***




XR (AR or VR) glasses is simulated with a computer and it is assumed that there is an AR glasses which look at the scenes and streams the video with specific ****resolution**** and ****frame rate**** (accordance with these features in the off-the-shelf XR glasses) to the edge server!
This XR simulated glasses can receive the scene with augmented digital object! (Full offloading). The experiment setup is shown in the following figure:

 ![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F18723381%2F9b91e482bc29c99457ec12b41790d4a2%2FAR%20Senario(60).png?generation=1708380734927241&alt=media)

We need to generate and stream the video from the scenes (as if user is looking at the environment) as a XR (AR or VR) glasses based on the specific frame rate and resolution as mentioned in the following table:

![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F18723381%2F7a3bd66e12f7e062465ab4c62aa62347%2FStreams.png?generation=1708380591528417&alt=media)

The seven environment frames, publish by Microsoft in https://www.microsoft.com/en-us/research/project/rgb-d-dataset-7-scenes/, are used to make it close to user experience!
Each sequence (seq-XX.zip) consists of 500-1000 frames. Each frame consists of three files:

- **Color**: frame-XXXXXX.color.png (RGB, 24-bit, PNG)

- **Depth**: frame-XXXXXX.depth.png (depth in millimeters, 16-bit, PNG, invalid depth is set to 65535).

- **Pose**: frame-XXXXXX.pose.txt (camera-to-world, 4×4 matrix in homogeneous coordinates).

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
 
 - ***[frame rate]*** -- > e.g. 30, 60, 90, 120 (fps)
 
 - ***[resolution]*** --> e.g. 1920x1080 

 - ***[sequence of png files]*** --> e.g. img%03d.png  (for the files with img001.png, img002.png, ... , img999.png)

 - ***[video name in mp4]*** --> e.g. my_video_1920_1080.mp4

 - ***libx264*** --> -vcodec libx264 is to set the encoding

**For example:**
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/14b47fb2-f9df-4383-a5b0-27fe29d9a45d)
**Output:**
![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/834385dc-0b05-4c88-81ed-b97b81f7f4a3)

**(1-1-3) Install gst-launch for video streaming [XR system]** 
[https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c]

    # sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio

**(1-1-4) Stream the video with specific resolution, frame rate, encoding and bitrate [XR system]**

    # gst-launch-1.0 -v filesrc location=./video1080_30.mp4 ! decodebin ! videoconvert ! videoscale ! video/x-raw,width=1920,height=1080 ! videorate ! video/x-raw,framerate=60/1 ! x264enc tune=zerolatency bitrate=5000 ! rtph264pay config-interval=1 pt=96 ! udpsink host=[IP address] port=[Port#]
    

- ***[location=./video1080_30.mp4]*** --> location of the video

- ***[width=1920,height=1080]***-->  resolution for streaming (This option can be neglected because it depends on the resolution of the video!)

- ***[framerate=60/1]*** --> frame rate of the streaming (This option can be neglected because it depends on the frame rate of the video!)

- ***[x264enc]*** --> The encoding which is H.264

- ***[bitrate=5000]*** --> It is the bitrate of sampling! (More bitrate higher sampling and higher video quality!)

- ***[rtph264pay]*** --> It is RTP protocol with H.264  encoding!

- ***[IP address]*** --> the edge server IP address e.g. 192.168.10.2

- ***[Port#]*** --> the port number e.g. 5000

**Output:**

![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/21206801/dbf7b664-52af-4d19-816d-5f155fb9058a)


## (2) CG traffic collection

To collect Cloug Gaming network telemtry data, we use a gadget between the CG server and clients (players). This gadget, called Raspberry Pi (having P4Pi system installed), runs a virtual switch and it can collect InBand Network Telemetry data and Packet Captures.

In the moment we are using only Xbox Cloud Gaming server in our experiments.

![image](https://github.com/dcomp-leris/VR-AR-CG-network-telemetry/assets/58492556/68a6c851-8863-43cd-aa0b-abb75a128d56)

### (2-1) Experiments

- Our experiments were made on two different network connections, **5G network** and **optical fiber wired connection**.
- We collected at the moment data about three diferent games, that are: Fortnite, Forza Horizon 5 and Mortal Kombat 11. 
- For each one, we played in one or two players.

We made different experiments switching those variables, and collected InBand Network Telemtry (INT) data, more especially the depth of the (virtual, emulated by Raspberry Pi) switch  queue of packets, and the timedelta that the packets stays in it. Beside that, we also collect pcap, using Raspberry Pi too.

### (2-2) Setup

Our setup is based in Raspberry Pi (Model 4), and one or two laptops.

For Raspberry Pi we installed P4Pi system, a plataform that allows to design and deploy network data planes written in P4 language using this gadget. You can know more about and find tutorials about how to install and manage it [here](https://github.com/p4lang/p4pi/wiki). P4Pi runs a virtual switch, and you can choose two different targets, T4P4S and BMv2. We use **BMv2**. After setting it, we created and deployed in BMv2 a P4 program able to parse our INT header in a packet, save all INT data in it, and then deparse the header and send the packet back to our host.

Our host is one of the laptops, and it runs two Python programs. The first one is responsible for creating INT packets (with INT header), and sending it to the host's network interface, one packet by second. The second program sniffs the network interface waiting for the INT packets, and, by each packet received, it get the fields that we need and save the values in our time series database. We are using [InfluxDB](https://www.influxdata.com/).




