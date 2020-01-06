
# Deploying Go Application to gcloud appengine

- web: https://stulast.co.uk
- url: https://github.com/StuLast/deploy-go-app-to-gcloud-appengine

## Notes on these instructions

- These instructions were written using a windows 10 computer with gitbash as the main command line tool.
- Any observations of differences for Mac/Linux will be greatly appreciated.

## Contents

- [Pre-requisites](#pre-requisites)
- [Setting Up The Project](#setting-up-the-project)
- [Configuring your app](#configuring-your-app)
- [Publishing your app to Google Clouds App Engine](#pushing-your-app-to-googles-cloud)
- [References](#references)

## Pre-requisites
-  Up-to-date gcloud SDK installed on your system:  https://cloud.google.com/sdk/install
-  Up-to-date gcloud App Engine Go Component installed on your system  [see "Installing gcloud App Engine Go Component"](#installing-gcloud-app-engine-go-component) 
-  gcloud project created and linked to [see "Setting up the project"](#setting-up-the-project)

### Installing gcloud App Engine Go Component

From the google cloud SDK Shell (started with admin permissions)

```
gcloud components install app-engine-go 
```

## Setting up the project.

### Creating a new project.

If you haven't yet created a gcloud project you can either do this from https://console.cloud.google.com  (see https://cloud.google.com/appengine/docs/standard/nodejs/building-app/creating-project) or from your computers command line using the SDK.  If you create the project via https://console.cloud.google.com you will need to carry out [linking to an existing project](#linking-to-an-existing-project). If, instead, you use the SDK command line on your computer, this will be done for you.

```
gcloud create project PROJECT-NAME
```

replacing PROJECT-NAME with the name of your own project.

### Linking to an existing project.

From any command line\terminal tool

```
gcloud config set project PROJECT-NAME
```

replacing PROJECT-NAME with the name of your own project.

*see https://cloud.google.com/sdk/gcloud/reference/config/set for more info on changing project settings

## Configuring your app.

The core of any Go app being hosted on google clouds App Engine consists of an  app.yaml file, which instructs google how to build the environment, and the main.go file, which contains the core code.

### app.yaml.

```
runtime: go113

handlers:
- url: /.*
  script: auto
  secure: always
```

- runtime describes the version of go to be used.
- handlers describes url patterns and how they should be treated.

for more information on the options available in app.yaml, visit https://cloud.google.com/appengine/docs/standard/go/config/appref

\* note that the app.yaml file has become more simplified with recent upgrades to the Google App Engine and the Go language.

### main.go

The main.go file is the main entry point for a Go program, and this is now no different for apps destined for publication on Googles App Engine.  Below is the minumum require code to serve static html files in Google App Engine.

```
package main

import (
	"net/http"
	"os"
	"log"
)

func main() {
	http.Handle("/", http.FileServer(http.Dir(".")))

	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
		log.Printf("Defaulting to port %s ", port)
	}

	log.Printf("listenting on port %s", port)
	if err := http.ListenAndServe(":"+port, nil); err != nil {
		log.Fatal(err)
	}
}
```

Note that we ask google to tell us what port it's assigning to our app.  For local development, our machine wont have a value for os.Getenv("PORT), so our if statement assigns 8080 (or whatever we tell it to).



## Pushing your app to Google's Cloud

Now that your local google environment has been set up to point to the correct App-Engine project, switch to your project folder (the one with app.yaml and main.go in it) and enter the following code in your local terminal/shell/cli to push your project to the project on you Google Cloud Account.

```
gcloud app deploy

```

To view your published app you can use the following code in your terminal/shell/cli

```
gcloud app browse

```
Or you can visit https://YOUR-PROJECT.appspot.com, replacing YOUR-PROJECTS with your own projects name.

We also make use of the core log library to record any errors.  You can monitor the log (which also shows all GET/POST requests), using the following code:

```
gcloud app logs tail -s default
```

Use Ctrl+C/Cmd+c to exit log tracking.


## References

Google has up to date guidance on publish Go based apps to the Google App Engine, available at https://cloud.google.com/appengine/docs/standard/go/building-app/




