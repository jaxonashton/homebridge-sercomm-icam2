<div id="top"></div>
<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

<div align="center">

<a href="https://homebridge.io"><img alt="Works with Homebridge" src="https://img.shields.io/badge/Works%20with-Homebridge-%23965DE1?style=for-the-badge"></a>
<a href="https://homebridge.io"><img alt="Works with Hoobs" src="https://img.shields.io/badge/Works%20with-Hoobs-%23F3B940?style=for-the-badge"></a>

<a href="#"><img alt="Project status" src="https://img.shields.io/badge/Status-Active-blue?style=for-the-badge"></a>
<a href="#"><img alt="GitHub repo stars" src="https://img.shields.io/github/stars/falc0n2k/speedtest-dashboard?style=for-the-badge"></a>
<a href="https://github.com/Falc0n2k/speedtest-dashboard/issues"><img alt="GitHub issues" src="https://img.shields.io/github/issues-raw/falc0n2k/speedtest-dashboard?style=for-the-badge"></a>
</div>

<br/>
<br/>

<h1 align="center">Sercomm iCamera 2 on Homebridge</h1>
<p align="center">Have a few Xfinity Home cameras but don't have Xfinity Home? Yes, you can still use them!</p>
</div>

<br/>
<br/>

<!-- ABOUT THE PROJECT -->
## About The Project

![Screenshot at Jun 05 20-58-35](https://user-images.githubusercontent.com/3246415/172081290-616ea0fc-635e-48bf-a955-8b76bb14c110.png)

### Dependencies

* [Homebridge](https://homebridge.io)
* Sunoo's [Camera FFmpeg plugin](https://github.com/Sunoo/homebridge-camera-ffmpeg#readme) for Homebridge
* A [Sercomm iCamera 2](https://user-images.githubusercontent.com/3246415/172081973-eeb4af69-61e9-4e49-ac70-6ebbaef80674.jpg) (aka 2nd Gen Xfinity Home Camera)
* Patience


<!-- GETTING STARTED -->
## Getting Started

If you had Xfinity Home at one time but no longer do, you might be surprised to learn that you own the equipment - the cameras, door/window sensors, etc (everything but the touchscreen, which you lease). The prevailing logic is that it's hard to leave a service when you've paid for the equipment outright and can't use it elsewhere... *or can you*?

This tutorial should take some of the pain out of re-purposing these otherwise solid cameras. Be mindful though: you're not going to get HD quality picure on these things, as the max resolution is 1280 x 720 (720p), and there's no ability to record the feed. However, all other functionality like night mode should still work.

Caution: If you're looking for a way to get your *working* Xfinity Home system integrated within HomeKit, check out the [Homebridge-XfinityHome](https://github.com/bloomkd46/homebridge-XfinityHome) by @bloomkd46. It's an excellent option to gain XH access within the HomeKit app, allowing you to ditch Xfinity's Home app. and keep everything in one place.

### Prerequisites

The first stop on getting the iCamera 2 set up for Homebridge integration is performing a factory reset on the device and manually tweaking some settings. Don't worry though, it's scarier than it sounds. You can read all about this process over on the [dependency primer](https://github.com/Falc0n2k/Homebridge-Sercomm-iCamera2/wiki/Dependency-Primer).

### Homebridge Config

    {
        "name": "[CAMERA_LOCATION_NAME]",
        "manufacturer": "Sercomm",
        "model": "iCamera2",
        "serialNumber": "[CAMERA_SERIAL_HERE]",
        "videoConfig": {
            "source": "-i rtsp://administrator@[CAMERA_IP]:554/img/media.sav",
            "stillImageSource": "[LEAVE_THIS_BLANK]",
            "audio": true
        }
    }

1. In Homebridge, click **Plugins** in the top menu. Locate the Homebridge Camera FFmpeg plugin and click **Settings**.

2. Start by naming your camera whatever you'd like, but remember that the name you choose is what it will appear as within HomeKit. I named my outdoor camera "Balcony".

3. The video source should always start with `-rtsp_transport tcp -i`, followed by the URL of the camera in the format of `rtsp://administrator@[CAMERA_IP]:[PORT_NUMBER]/img/media.sav`. This will call to the camera and force streaming over TCP to be then handled by the FFmpeg plugin.

3. Leave the "Still Image Source" blank. I originally thought that this had to be filled in based on the very detailed technical dive provided by edent in their [Sercomm-API](https://github.com/edent/Sercomm-API) repo, however, as it turns out leaving it blank is the way to go since will pull a snapshot from the live feed every 15-30 seconds.

4. Make sure **Enable Audio** and **Unbridge Camera** are both checked.

   At this point, your screen should look something similar to this:

   ![Screenshot at Jun 05 21-43-58](https://user-images.githubusercontent.com/3246415/172081455-86926f9c-9b6c-43d7-9095-9a5a905f4770.png)

5. The following options are just that -- optional. They do not impact the functionality of the camera, but they sure make it easier to troubleshoot especially if you have more than one iCam.

    **Branding Options**

    1. Manufacturer: `Sercomm`
    2. Model: `iCamera2`
    3. Serial Number: `can be found at XYZ`
    4. Firmware Revision: `leave blank`

    ![Screenshot at Jun 05 21-44-33](https://user-images.githubusercontent.com/3246415/172081476-cb7bac7f-5b41-49c2-86ce-5fc2597fc5c2.png)

    **Video Output**

    1. Maximum Concurrent Streams: `2`
    2. Maximum Width: `0`
    3. Maximum Height: `0`
    4. Maximum Framerate: `30`
    5. Maximum Bitrate: `299`
    6. Force Maxmiums: `leave unchecked`

    ![Screenshot at Jun 05 21-44-59](https://user-images.githubusercontent.com/3246415/172081491-1a69bb42-30fc-4df1-b8f8-f3e201d62663.png)

6. I do not recommend playing with any of the options in the "EXPERIMENTAL - WIP" section as you can really mess up your setup.

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* edent's [Sercomm Camera API](https://github.com/edent/Sercomm-API) documentation
* Sunoo's [Homebridge Camera FFmpeg plugin](https://github.com/Sunoo/homebridge-camera-ffmpeg#readme)

