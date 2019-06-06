# Your IIIF ecosystem

How to configure local IIIF-related servers, based primarily on information from <https://iiif.github.io/training/iiif-5-day-workshop/>.

## Overview

### Core components

1. Make images available on a IIIF image server, such as Cantaloupe. This server must be running.
2. Edit manifests in the Bodleian IIIF manifest editor. This server can be shut down after the manifests are completed.
2. Make manifests available on a regular HTTP server, such as the npm http server (locally) at <https://www.npmjs.com/package/http-server> or your regular web server. This server must be running.
3. Access IIIF data in a IIIF viewer, such as Mirador.

### Ancillary components

1. Access the image API directly, bypassing the presentation API.
2. Build a manifest automatically for an entire directory of images.
3. Create tiles for level 0 static images.
4. Validate a manifest.

## Prerequisites

1. Install latest versions of Chrome and Firefox.
1. Install JSONView extension in Chrome from <>.
2. Install Node.js from <https://nodejs.org/en/>.

## Image server: Cantaloupe

Install Cantaloupe (<https://cantaloupe-project.github.io/>). Make the following changes:

1. Move into your Cantaloupe directory.
2. Copy the configuration file with `cp cantaloupe.properties.sample cantaloupe.properties`.
2. If you are running locally (that is, if you don’t need to be concerned about intrusions), beginning at line 104, make the following changes:

	```
	# Enables the Control Panel, at /admin.
	endpoint.admin.enabled = true
	endpoint.admin.username = admin
	endpoint.admin.secret = yolo
	```
3. Launch with `$ java -Dcantaloupe.config=./cantaloupe.properties -Xmx2g -jar Cantaloupe-4.1.2.war`	(change the version number to match your *war* file). 
4. Change the http and https ports in the configuration file, if desired.
4. Access at <http://127.0.0.1:8182/iiif/2> (or alternative port).
5. If you have enabled the admin dashboard, access it at <http://127.0.0.1:8182/admin> (or alternative port).
6. To configure the image directory, in the admin dashboard click on *Source* and then *FilesystemSource*. The image directory does not have to be in the Cantaloupe hierarchy, but something like *./images/* would work (note that the trailing slash is required). If you have not enabled the admin dashboard, you can edit *cantaloupe.properties* manually, changing line 139 to:

	```
	FilesystemSource.BasicLookupStrategy.path_prefix = ./images/
	```
to point to images located under your main Cantaloupe directory (or use another location).

Examine hosted images with the following online browser resources (note that Leaflet and OpenSeaDrag want the *info.json* file, while the UCD Image clipper wants the image resource itself):

1. Leaflet: <http://mejackreed.github.io/Leaflet-IIIF/examples/?url=http://mejackreed.github.io/Leaflet-IIIF/examples/?url=http://localhost:8182/iiif/2/eddie.jpg/info.json>
2. OpenSeaDragon: <http://iiif.gdmrdigital.com/openseadragon/index.html?image=http://localhost:8182/iiif/2/eddie.jpg/info.json>
3. UCD Image clipper: <https://jbhoward-dublin.github.io/IIIF-imageManipulation/index.html?imageID=http://localhost:8182/iiif/2/eddie.jpg>

The image API is documented at <https://iiif.io/api/image/2.1/>.

## Bodleian manifest editor

The Bodleian manifest editor can be installed and run locally—instructions at <https://github.com/bodleian/iiif-manifest-editor>. But use Firefox; Chrome blocks access to remote URIs (grr!). 

Runs by default on port 3000; change in *server.js* if desired. Once it is installed, launch it by moving into the repo and running `npm run start`.

The presentation API is documented at <https://iiif.io/api/presentation/2.1/>.

## HTTP server

Two options are the *npm http-server* and the *Chrome server*.

### npm http-server

1. Install the server with `npm install -g http-server`.
2. Launch in the directory you intend to use as your web root (e.g., above your manifest) with `http-server --cors`. Defaults to port 8080; specify an alternative port with `http-server -p 1234 --cors`.
3. Visit at <http://localhost:1234>.

### Chrome server

1. Follow the instructions at <https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en> to install the Chrome server. 
2. In the pop-up, check the box to enable CORS, set the web root, and change the port, if desired.

## HTTP viewer: Mirador

1. Download *build.zip* for the latest stable release of Mirador at <https://github.com/ProjectMirador/mirador/releases>.
2. Unzip *build.zip*.
3. Inside the new *build* directory, open *example.html* in a browser.
4. To add a new manifest to the inventory Mirador knows about:
	1. Click on the plus sign in the middle to “Add item”.
	1. Look in the upper left, in the box labeled “addNewObject:”. 
	1. Enter the http or https (not filesystem) URL of your manifest, e.g., <http://127.0.0.1:1234/pmb_manifest.json>.
	1. Click “load”. Your manifest should be added to the list of those available.
	1. Click on the new link to your manifest to view your images.
2. To make the addition stable, edit *example.html* to add your the http or https URI for your manifest, following the pattern of the resources already listed there.

#### About CORS

The server hosting your manifest must be configured for CORS. This is an HTTP server configuration issue that goes beyond IIIF, but see <https://ronallo.com/iiif-workshop/bonus/cors.html> for some information. On my Apache server I did the following:

1. Ensure that headers are enabled, that is, that the line in the server configuration file that reads `LoadModule headers_module modules/mod_headers.so` is not commented out. It should be enabled by default.
2. Ensure that *.htaccess* directives are supported with `AllowOverride All`. This should be the default.
3. Create an *.htaccess* file in the root directory of the web subtree that hosts your manifests, with the following content:

	```
	<IfModule mod_headers.c>
    	Header set Access-Control-Allow-Origin "*"
	</IfModule>
	```

## Bypass the IIIF presentation API

The manifest provides access to presentational metadata that may be useful, but the IIIF image server can be used without a IIIF image viewer, providing access to the images in a way that utilizes the image API. To do this, add links in your HTML page the point to IIIF resources using the IIIF image API syntax, e.g., <https://stacks.stanford.edu/image/iiif/hv814gh0574%2F2515057/full/full/0/default.jpg>.

## Build a manifest automatically for an entire directory of images

Download and run Jeff Witt’s ruby script at <http://jeffreycwitt.com/IIIFWorkshop/docs/doc3#method-2-create-a-script> in your image directory.

## Create tiles for level 0 static images

Perform a *manual installation* for <https://github.com/zimeon/iiif>. Instructions for creating tiles are at <https://github.com/zimeon/iiif/tree/master/demo-static>.

## Validate a manifest

To validate locally, install and run the Tripoli IIIF manifest validator (Python package) by following the instructions <https://github.com/DDMAL/tripoli>.

If your manifest is available at a public URL, you can validate it on line at <https://iiif.io/api/presentation/validator/service/>. 

## Annotations

### Simple annotation server

Install the simple annotation server by following the instructions at <https://iiif.github.io/training/iiif-5-day-workshop/day-three/annotations-stores-install.html>. The steps (copied from this site) are:

1. Download *sas.zip* from <https://github.com/glenrobson/SimpleAnnotationServer/releases>. Unzip and `cd` into the directory. 
2. Launch with `java -jar dependency/jetty-runner.jar simpleAnnotationStore.war`. It defaults to port 8080; to specify an alternative port, use `java -jar dependency/jetty-runner.jar --port 9090 simpleAnnotationStore.war`
1. Launch with `http://localhost:8080/index.html`, which starts a customized version of Mirador 2.
2. Make a manifest available locally (e.g., in the Chrome HTTP server, at *http://localhost:8887/manifest.json*. Set the manifest-level `@id` value of that manifests to the URI at which it can be accessed, e.g., for the example above, "http://localhost:8887/manifest.json".
3. Add this manifest to the simple-annotation-server version of Mirador.
4. Add annotations to the manifest using the Mirador annotation interface.
5. Navigate to the upload interface of the simple annotation server at *http://localhost:8080/uploadManifest.html* and upload the manifest (e.g., "http://localhost:8887/manifest.json"). This opens a page in the web server, from which you should copy the address, e.g., "http://localhost:8080/search-api/localhost:8887/search".
6. Add the following section to the manifest after the manifest-level `label` (don’t forget the comma):

	```
	"service": {
	    "profile": "http://iiif.io/api/search/0/search",
	    "@id": "http://localhost:8080/search-api/localhost:8887/search",
	    "@context": "http://iiif.io/api/search/0/context.json"
	},
	```
The `@id` value must be the URI you copied in the preceding step.
1. Test the result in the Universal viewer by entering your manifest URI in *http://universalviewer.io/*. It should render a search-box (case sensitive).

### Hosting annotation locally

ADD description from <https://github.com/glenrobson/SimpleAnnotationServer/blob/master/doc/DownloadAnnotations.md>. 

____

## To do

* IIIF annotation building tool: <https://ncsu-libraries.github.io/iiif-annotation/>.
