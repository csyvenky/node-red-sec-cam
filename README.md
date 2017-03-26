# node-red-sec-cam
node-red Security Camera Project for Raspberry Pi
![Node-Red GUI](https://raw.githubusercontent.com/csyvenky/node-red-sec-cam/master/nr-gui.PNG "Node-Red GUI")

## Project Goals
- to have a couple Raspberry Pi constantly taking home security photos.
- uploading the photos to cloud-based blob storage service; I ended up with AWS but Azure node worked nicely too.
- to persist the image only if it has a quality worth keeping (ie. no black images at night).
- to purge old images; prevent the system from just filling up the drive.
- to save some operational information; I wanted to confirm the system was running correctly.
- gather statistical reporting numbers; tallying the storage being consumed.

## System Components/Requirements
- Raspberry Pi w/ Camera Module
- Python PiCamera Module 
- ImageMagick Software Package
- Node-Red
- AWS Account
- (optional) Splunk for Operational Reporting

## System Flow
- Python script takes photo every 30 seconds and saves it locally to Pi drive.
- Node-Red flow watches the folder for changes.
- Prior to saving the image to cloud storage the ImageMagick identify application is called in order to process the image's blackness value - if the image is too dark it will not be saved.
- If the image quality is good the flow to AWS blob-storage node is used.
- In parallel to the AWS save the operational information is saved to a local Splunk infrastructure.
- The Node-Red flow also has a sub flow that periodically deletes old images from the Pi and saves the details to Splunk.
- Finished.

The Node-Red project is on [GitHub](https://github.com/csyvenky/node-red-sec-cam)

![Splunk Dashboard](https://raw.githubusercontent.com/csyvenky/node-red-sec-cam/master/splunk-dashboard.PNG "Splunk Dashboard")
