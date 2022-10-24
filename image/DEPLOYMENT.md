# Deployment guide

## Background

We are using two Docker images to build the Karota webapp, these images are called 'base_slim' and 'base'. We're gonna build the images in our local machine first then we will upload the images to the GCP Container Registry so that the images can be pulled inside the VM instance.

## Prerequisite

Make sure that the Google Cloud authentication is properly setup. Review your configured email and project name in `~/.config/gcloud/configurations/`.

## Step-by-step deployment guide

1. Change directory to `/image`
2. Build the 'base_slim' image by running the command below.
	```
	$ make build-slim
	```
3. After building the image, we will automatically tag the image and upload it to the GCP Container registry by running the command below. Tip: you can check the image basic info by running `docker images`.
   ```
	 $ make upload-slim
	 ```
4. Build the 'base' image by running the command below.
	```
	$ make build-base
	```
5. Then tag the image and upload it to the GCP Container registry by running the command below.
	```
	$ make upload-base
	```

# Rebuilding the app in the VM instance

To be able for the changes to take effect in the webapp, we need to rebuild the docker container in the GCP VM instance. Connect through the VM instance and run this commands:

```
$ sudo -s
$ cd /var/discourse
$ ./launcher rebuild app
```