# chime-to-rtmp

This repository contains a Docker container that, when started, will join an Amazon Chime meeting by PIN and broadcast the meeting's audio and video in high definition (1080p at 30fps) to an RTMP endpoint you specify. The broadcast participant joins the meeting in the muted state. The meeting PIN must be unlocked in order for the broadcast participant to join the meeting.

## EC2 Deployment:

## Prerequisites

You will need Docker and `make` installed on your system. As this container is running a Firefox browser instance and transcoding audio and video in real time, it is recommended to use a host system with at least 8GB RAM and 4 CPU cores, such as an m5.xlarge EC2 instance running Ubuntu Linux 18.04 LTS.
 
## Configuration

The input for the container is a file called `container.env`. You create this file by copying the `container.env.template` to `container.env` and filling in the following variables:
 
 
* `MEETING_URL`: Chime Meeting URL (without any spaces in it)
  * Example(If you want to record Chime): `https://app.chime.aws/portal/<your Meeting ID here>`
  * Example(Hosted [Chime SDK Serverless Demo](https://github.com/aws/amazon-chime-sdk-js/tree/master/demos/serverless) URL): `<Hosted Chime URL>/?m=<Meeting ID>&broadcast=true`
* `RTMP_URL`: the URL of the RTMP endpoint,
  * Twitch example: `rtmp://live.twitch.tv/app/<stream key>`
  * YouTube Live example: `rtmp://a.rtmp.youtube.com/live2/<stream key>`

## Running

To build the Docker image, run:
 
```
$ make
```
 
Once you have configured the `container.env` file, run the container:
 
```
$ make run
```
 
The container will start up and join the given Amazon Chime meeting as the `<Broadcast>` attendee and start streaming H.264/AAC in FLV format to the given RTMP endpoint.

When your broadcast has finished, stop the stream by killing the container:

```
$ docker kill bcast
```

If you launched an EC2 instance to host the Docker container, you may also want to stop the instance to avoid incurring cost.

## ECS Deplyoment:

## Prerequisites using Windows 10

You will need the following on your local Machine
1. Download and Install Copilot
2. Download and install Docker Desktop for Windows. Run Docker. Make sure WSL2 Linux kernal package 64 bit is installed on your Windows PC. (this is needed to run docker on Windows) 
3. Install Git for Windows
4. Install AWS CLI
5. Windows Powershell

## Configuration

* Create a new folder on your local drive
* Clone this Github repository into the newly created folder
* Go to your AWS Account, IAM -> users -> security credentials, create and access key, and download the .csv file in a known location on your local Machine. (if not done already)
*   

