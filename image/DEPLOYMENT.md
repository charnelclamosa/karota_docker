# Deployment guide

## Background

We are using two Docker images to build the Karota webapp, these images are called 'base_slim' and 'base'. We're gonna build the images in our local machine first then we will upload the images to the GCP Container Registry so that the images can be pulled inside the VM instance.

## Prerequisite

1. Make sure that the Google Cloud authentication is properly setup. Review your configured email and project name in `~/.config/gcloud/configurations/`.
2. Updated git tag for [karota_webapp repository](https://github.com/zatonovo/karota_webapp), this git tag will be used as tag of Docker images that will be uploaded in Google Cloud Container Registry.

## Step-by-step deployment guide

1. Change directory to `/image`
2. Execute the command below:

	```
	$ make deploy
	Or
	$ sudo make deploy
	```

This command will build these Docker images:

1. discourse/base:slim
	Contains all the required bundles and libraries to run our webapp. The GitHub repository is cloned on this image.
2. discourse/base:base
  This image is using the `discourse/base:slim` image as the base image. This image is a production-ready image.
1. gcr.io/karota-uat/karota_base_slim:[latest, {git tag}]
  This image is derived from `discourse/base:slim`, this will be automatically uploaded to the Google Cloud Container Registry, two of this image will be uploaded, the first one is with tag of `latest` and the latter is the latest git tag of [karota_webapp repository](https://github.com/zatonovo/karota_webapp).
1. gcr.io/karota-uat/karota_base:[latest, {git tag}]
  This image is derived from `discourse/base:base`, this will automatically uploaded to the Google Cloud Container Registry, two of this image will be uploaded, the first one is with tag of `latest` and the latter is the latest git tag of [karota_webapp repository](https://github.com/zatonovo/karota_webapp).


# Rebuilding the app in the VM instance

To be able for the changes to take effect in the webapp, we need to rebuild the docker container in the GCP VM instance. Connect through the VM instance and run this commands:

```
$ sudo -s
$ cd /var/discourse
$ ./launcher rebuild app
```

# Slite documentation

For the Slite version of this documentation, visit this link: https://zatonovo.slite.com/app/docs/RF2gb8BfT6-TVE/Deployment