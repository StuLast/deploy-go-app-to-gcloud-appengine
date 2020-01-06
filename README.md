
# Deploying Go Application to gcloud appengine

- web: https://stulast.co.uk
- url: https://github.com/StuLast/deploy-go-app-to-gcloud-appengine

## Notes on these instructions

- These instructions were written using a windows 10 computer with gitbash as the main command line tool.
- Any observations of differences for Mac/Linux will be greatly appreciated.

## Contents

- [Pre-requisites](pre-requisites)
- [Setting Up The Project](setting-up-the-project)
- [Configuring your app](configuring-your-app)

## Pre-requisites
-  Up-to-date gcloud SDK installed on your system:  https://cloud.google.com/sdk/install
-  Up-to-date gcloud App Engine Go Component installed on your system  [see "Installing gcloud App Engine Go Component"](Installing-gcloud-App-Engine-Go-Component) 
-  gcloud project created and linked to [see "Setting up the project"](setting-up-the-project)

### Installing gcloud App Engine Go Component

From the google cloud SDK Shell (started with admin permissions)

``` gcloud components install app-engine-go ```

## Setting up the project.

### Creating a new project.

If you haven't yet created a gcloud project you can either do this from https://console.cloud.google.com  (see https://cloud.google.com/appengine/docs/standard/nodejs/building-app/creating-project) or from your computers command line using the SDK.  If you create the project via https://console.cloud.google.com you will need to carry out [linking to an existing project](linking-to-an-existing-project). If, instead, you use the SDK command line on your computer, this will be done for you.

```gcloud create project PROJECT-NAME```

replacing PROJECT-NAME with the name of your own project.

### Linking to an existing project

From any command line\terminal tool

```gcloud config set project PROJECT-NAME```

replacing PROJECT-NAME with the name of your own project.

*see https://cloud.google.com/sdk/gcloud/reference/config/set for more info on changing project settings

## Configuring your app

