# IIIF: DHSI 2019

## 2019-06-03

### Materials

* Git book: <https://iiif.github.io/training/iiif-5-day-workshop/>
* Course pack: <http://dhsi.org/content/2018Curriculum/24.%20Introduction%20to%20IIIF-%20Sharing,%20Consuming,%20and%20Annotating%20the%20World%E2%80%99s%20Images.pdf>
* Shared notes:  <http://bit.ly/dhsi-iiif-19>

### Instructors 

* Josh Hadro, IIIF consortium Managing Director 
* Glen Robson, IIIF consortium Technical Coordinator
* Peter Broadwell, Stanford University Libraries
* Camille Villa, Stanford University Libraries
* Jennifer Vine, Stanford University Libraries (UI designer for Mirador, as observer)

### Housekeeping

#### Ports to use

* Cantaloupe 8143
* NPM http-server 8080 (no; port in use)
* Chrome webserver 8887
* SimpleAnnotationServer 8888 (no; port in use)

IIIF DHSI 2019 Slack: <https://iiif.slack.com/>. Channel #dhsi-2019 (private): josh.hadro, glen.robson, camillevilla, peter_broadwell

### Overview

#### Definition

Internation Image Interoperability Framework. Expanded version of one-day workshop. 

Conceptually, IIIF is a *model* for presenting and annotating digital representations of objects. As a community, it develops *shared APIs* (“computers talking to computers”, in a way that makes it easy to swap out components), implements them in *software*, and exposes *interoperable* software. Also works with audio and video *time-based* media.

The two main APIs are the *image API* (pixels) and the *presentation API* (minimal metadata to drive viewing experience). Common view might have large single image, thumbnails of all related images for browsing, and contextual information in a sidebar. Thumbnails and sidebar are presentation API, while the image (and the images within the thumbnail panel) are image API. Other APIs include search (full text) and authentication (login, differential access). 

#### Features

* Deep zoom with large (both bytecount and physical size) images through the use of tiling. 
* Cite and share, including regions
* Compare (juxtapose or overlay) different views of the same material
* Integrate materials from different repositories seamlessly, including fragments (implemented in *manifest.json*)
* Search within transcriptions of images (specific IIIF search API)
* Overlay digitized authentic maps and georeference them
* Annotate regions of an image (pop-ups)
* Interchangeable applications (e.g., Mirador, Universal Viewer, IA Book Reader viewers). Qatar National Library <http://labs.cogapp.com/iiif> lets user choose among four viewers.

#### Examples

Links at <https://hadro.github.io/presentations/dhsi2019/>. 

* Miranda Search Tool, <https://collections.folger.edu/>. Viewer opens automatically in catalog record, with option to open dedicated viewer.
* Indigenous digital archive, Museum of Indian Arts and Culture <https://omeka.dlcs-ida.org/s/ida/page/home> (click into collection); performs transcription.
* Brooklyn Public Library city directories <http://iiif.archivelab.org/iiif/1885BPL>
* North Carolina State University search integration, e.g., *Nubian message* (local publication). 
* Life of the Buddha <http://lotb.iath.virginia.edu/mirador_viewer/mirador?manifest=1&room=1> includes annotations of large mural image. 
* <https://chronicle250.com/1822#art+criticism+-+landscape+painting>
* Digiratei Timeliner (<https://github.com/digirati-co-uk/timeliner>) annotates audio materials in time dimension. 

### Image API

#### Overview

Zooming out (<https://docs.google.com/presentation/d/1kN1mt8rmILTcVnfBPjef1F3YvL7dI6MtjmiHv6840Gk/edit#slide=id.p>). Explains API concept. IIIF image API specifies, in order as part of a URI, 1) image, 2) region, 3) size, 4) rotation, 5) quality (color, etc.), and 6) format. E.g., _http://www.example.org/image-service/abcd1234/80,15,60,75/pct:80/345/grey.jpg._ The server can deliver both the image related metadata. Servers listen on ports. 

#### Install the following

* JSONView extension for Chrome. After installation, open <https://purl.stanford.edu/fr426cg9537/iiif/manifest>. You should see: 1) text in two different colors and 2) arrows on the lefthand side that allow you to collapse / expand sections.
* Web Server for Chrome (<https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en>). 
* Java 8 or higher. You may need to restart your computer afterwards. <https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>
* Git.
* Node. Select the version labeled “Recommended for most users” (<https://nodejs.org/en/>). In your command line, type `node -v` and `npm -v` to verify installation.

#### First steps

Slides: <https://docs.google.com/presentation/d/1sannNtwGz-h9oRe02tZ2aOjr7jJ9LYY6W_jP3uvA9kk/edit#slide=id.p>

Three options for serving: hosted service, Cantaloupe, static (free hosted service). 

Image and presentation APIs can be used independently of each other, but they are typically the core of a IIIF setup (search, authentication, annotation are later). 

IIIF servers take over for METS servers, which did the same work, but were not standardized. In the older architecture, there were sepaarate thumbnail, web, and archival images. Limitations: web copy resolution was too low, mobile required still other resolutions, so moved to zoomable images (Flash-based Zoomify). These were problematic because, insofar as they were Flash-based, didn’t work on mobile and didn’t support download. File formats were proprietary (e.g., pff format for Zoomify), which meant lock-in.

Desiderata for IIIF:

* Image at different sizes, but ...
* ... only one file
* Mobile access
* Format agnostic, supporting open image formats
* Zooming, but interchangeable viewers
* Pick and choose viewer
* Citeable URL
* “Super slippy viewer” = satisfying viewing experience

#### Links

* Outline: <https://iiif.github.io/training/iiif-5-day-workshop/day-one/image-api.html>
* Slides: <https://docs.google.com/presentation/d/1sannNtwGz-h9oRe02tZ2aOjr7jJ9LYY6W_jP3uvA9kk/edit?usp=sharing>
* Play with Image API parameters: <https://tomcrane.github.io/the-long-iiif/image-api.html>, <http://tomcrane.github.io/presentations/tile-exploder.html>
* Parameter explanations: <https://iiif.io/api/image/2.1/#canonical-uri-syntax>

#### *info.json*

The system knows what’s available through *info.json*. Tiles are usually square (usually 256 or 512). Scale factors control how far it can zoom in. Sizes are cached requestable sizes (you can request any size, but cached are much faster). Formats, qualities, and functionality (supports). Levels are degree of support: 0 only specified sizes and tiles; 1 = any, 2 = all of the above and more.

#### Exercises

* Have a look at Jason Ronallo’s ‘Detailed Example’, <http://ronallo.com/iiif-workshop/image/detailed-example.html>.
* Play with Tom Cranes examples: <https://tomcrane.github.io/the-long-iiif/image-api.html>
* Have a look at the UCD image cropper: <https://jbhoward-dublin.github.io/IIIF-imageManipulation/index.html?imageID=https://iiif.ucd.ie/loris/ivrla:10408>
* In pairs do the gitbook exercises (2.3 “Image API”; <https://iiif.github.io/training/iiif-5-day-workshop/day-one/image-api.html>).
* If you finish have a look at: <http://puzzle.mikeapps.me/>.

#### Tasks

* Given the following image URL: <https://iiif.lib.ncsu.edu/iiif/mc00198-008-ff0051-000-001_0001/full/512,/0/default.jpg>, what would be the URL for the *info.json*? Answer: <https://iiif.lib.ncsu.edu/iiif/mc00198-008-ff0051-000-001_0001/info.json>
* What is the width and height of the biggest size image? Answer: "width": 5947,
"height": 5088 (these are both width and maxWidth and height and maxHeight. Typically specified width and height; server must be configured to support `sizeAboveMax` to go above (most aren’t). 
* Construct a URL that: Shows the full image, is 512px wide, is upside down, gray scale, has a format of png. Answer: <https://iiif.lib.ncsu.edu/iiif/mc00198-008-ff0051-000-001_0001/full/512,/180/gray.png>
* What formats can you request for this image? Answer: jpg, png
* Looking at the IIIF API Specification <https://iiif.io/api/image/2.1/>, what status code should be returned if you ask for a format the image server doesn't support? Answer: 400 (<https://iiif.io/api/image/2.1/#format>)
* What does max image size mean? How does it differ to full? When might you use this? Answer: “The image or region is returned at the maximum size available, as indicated by maxWidth, maxHeight, maxArea in the profile description. This is the same as full if none of these properties are provided.” That is, full is maximum *possible*, while full is maximum *allowable*; the server can permit zooming to full resolution without supporting downloading it (doesn’t really prevent downloading at highest-level tiles and stitching together). Full size is deprecated and will be removed; max (new in 2.1) will be the only supported alternative for “as much as I can get”.
* Given the following info.json:

```json
{
    "@context": "http://iiif.io/api/image/2/context.json",
    "@id": "http://example.com/iiif/image/2",
    "@type": "iiif:Image",
    "protocol": "http://iiif.io/api/image",
    "width": 400,
    "height": 400,
}
```

and ![image](https://iiif.github.io/training/iiif-5-day-workshop/images/4_Quadrants.jpg):

HINT: remember 0,0 is the top left of the image.

* What would the URL be to cut out the green quarter? Answer: <http://example.com/iiif/image/2/0,0,200,200/full/0/default.jpg>
* What would the URL be to cut out the white quarter? Answer: <http://example.com/iiif/image/2/200,200,200,200/full/0/default.jpg>.

Bonus points: Given the following info.json: <https://iiif.lib.ncsu.edu/iiif/mc00198-008-ff0051-000-001_0001/info.json>, how many tiles would you need to show the full image? Width is 5907, tile width is 1024 (and it’s square), so divide width and height by 1024 and multiply: 5.8 * 4.9 = (round up) 6 * 5 = 30 tiles.