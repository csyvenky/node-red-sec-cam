# node-red-sec-cam
node-red Security Camera Project for Raspberry Pi
![Node-Red GUI](https://raw.githubusercontent.com/csyvenky/node-red-sec-cam/master/nr-gui.PNG "Node-Red GUI")

## Project Goals
- to have a couple Raspberry Pi constantly taking home security photos.
- uploading the photos to cloud-based blob storage service; I ended up with AWS but [Azure Blob Storage Node](https://flows.nodered.org/node/node-red-contrib-azure-blob-storage) worked nicely too.
- to persist the image only if it has a quality worth keeping (ie. no black images at night).
- to purge old images; prevent the system from just filling up the drive.
- to save some operational information; to confirm the system was running correctly.
- gather statistical reporting numbers; tallying the storage being consumed.

## System Components/Requirements
- [Raspberry Pi](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) w/ [Camera Module](https://www.raspberrypi.org/products/camera-module-v2/)
- [Python PiCamera Module](https://picamera.readthedocs.io/en/release-1.13/)
- [ImageMagick Software Package](https://www.imagemagick.org/script/index.php)
- [Node-Red](https://nodered.org/)
- [Amazon Web Services](https://aws.amazon.com/) Account
- (optional) [Splunk](https://www.splunk.com/) for Operational Reporting

## System Flow
- Python script takes photo every 30 seconds and saves it locally to Pi drive.
- Node-Red flow watches the pictures folder for changes.
- Prior to saving the image to AWS S3 cloud storage the ImageMagick "identify" application is called in order to process the image's blackness value - if the image is too dark it will not be saved.
- If the image quality is good the flow saves the photo to AWS S3 blob-storage.
- S3 policies move older photos to Glacier storage to save on costs; an additional policy will purge/expire the images from AWS.
- In parallel to the save operation the system saves operational metadata to a local Splunk infrastructure.
- The Node-Red flow also has a sub-flow that periodically deletes old images from the Pi and saves the details to Splunk.
- Finished.

The Node-Red project is on [GitHub](https://github.com/csyvenky/node-red-sec-cam)

![Splunk Dashboard](https://raw.githubusercontent.com/csyvenky/node-red-sec-cam/master/splunk-dashboard.PNG "Splunk Dashboard")
