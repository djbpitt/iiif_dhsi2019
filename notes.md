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


