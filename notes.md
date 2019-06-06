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

* Cantaloupe 8143 (version 4.1.2 defaults to 8182)
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
* What does max image size mean? How does it differ to full? When might you use this? Answer: “The image or region is returned at the maximum size available, as indicated by maxWidth, maxHeight, maxArea in the profile description. This is the same as full if none of these properties are provided.” That is, full is maximum *possible*, while full is maximum *allowable*; the server can permit zooming to full resolution without supporting downloading it (doesn’t really prevent downloading at highest-level tiles and stitching together). Full size is deprecated and will be removed; max (new in 2.1) will be the only supported alternative for “as much as I can get”. Unclear whether specifying full in 3.0 will break or just return max.
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

Most people use Jpeg2000 (jp2) and Pyramid TIFF, which are pyramid image formats that store all sizes, pregenerated, within file. These are faster to process because the conversions are cached. Pyramid TIFF is the fastest of all. (Jpeg2000 is compressed and needs time to uncompress), but too large to be practical for most users, so Jpeg2000 predominates, at least for delivery. Pyramid image formats satisfy the desire for managing only one file, and mobile access is now well supported by JavaScript. Format-agnostic is more about the image server than the image API.

### Using the image API

#### Hosted

* Klokan: <https://www.iiifhosting.com/>
* Digirati: <https://dlcs.info/>

How to integrate image serving? See what your institution supports. Some embed in regular CMS. Some Digital Asset Management systems support (Rosetta ExLibris, Content DM OCLC, Luna Imaging); so do some repositories (Fedora DSpace). Or use a hosting solution like Klokan (only image API) or Digirati (image and metadata), both of which host IIIF images, both of which charge beyond small, sample data.

#### Static tiles

* Code <https://github.com/zimeon/iiif/tree/master/demo-static>
* Example: <https://glenrobson.github.io/iiif/2018/01/12/iiif-from-scrtach.html> (sic)

Level 0 image servers don’t require a server, but split the image into tiles. No custom processing, but can request what has been pregenerated, so zooming is supported, but not as fully as an image server. Free, fast (no processing), scalable. Disadvantages are that it’s harder to use (can be configured through GitHub hosting) and not all viewers work with level 0 images (although all should). 

#### IIIF Image server

* <https://github.com/IIIF/awesome-iiif#image-servers>
* Main ones:
	* IIP Image - written in C, <https://iipimage.sourceforge.io/>
	* Loris - python, <https://github.com/loris-imageserver/loris>
	* Cantaloupe - Java, <https://cantaloupe-project.github.io/>

Most functionality, but highest demand on resources (server).

#### Viewers

Basic zooming viewers, but without access to all metadata.

* Open Sea Dragon, <https://openseadragon.github.io/>
* Leaflet, 

### IIIF hosting software as a service

<https://iiif.github.io/training/iiif-5-day-workshop/day-one/iiif-hosting-saas.html>

Follow instructions to upload image to free hosting, and to view in the following viewers. Scheme (http ~ https) must match; use https for both server and image. Apparently the first two want the *info.json* URI; the last doesn’t. 

* Leaflet: <https://mejackreed.github.io/Leaflet-IIIF/examples/?url=>
e.g. <https://mejackreed.github.io/Leaflet-IIIF/examples/?url=https://free.iiifhosting.com/iiif/e638d12d9c771225a076efdc3cc7bef628fd28c759a0bd393b9f133223b48753/info.json>
* OpenSeaDragon: <https://iiif.gdmrdigital.com/openseadragon/index.html?image=>, e.g., <https://iiif.gdmrdigital.com/openseadragon/index.html?image=https://free.iiifhosting.com/iiif/e638d12d9c771225a076efdc3cc7bef628fd28c759a0bd393b9f133223b48753/info.json>
* UCD Image clipper: <https://jbhoward-dublin.github.io/IIIF-imageManipulation/index.html?imageID=>, e.g., <https://jbhoward-dublin.github.io/IIIF-imageManipulation/index.html?imageID=https://free.iiifhosting.com/iiif/e638d12d9c771225a076efdc3cc7bef628fd28c759a0bd393b9f133223b48753/>.

## 2019-06-04

Install Cantaloupe following the instructions at <https://iiif.github.io/training/iiif-5-day-workshop/day-one/setting-up-cantaloupe.html> and then configure as per <https://iiif.github.io/training/iiif-5-day-workshop/day-one/configuring-cantaloupe.html>.

Small but important details:

* On day 1 we were advised to use port 8143 for Cantaloupe, but today’s instructions default to 8182. Rely on 8182 and forget about 8143.
* The link in the GitBook to the OpenSeaDragon viewer has a typo (double equal sign where there should be a single). Correct it to: `http://iiif.gdmrdigital.com/openseadragon/index.html?image=http://127.0.0.1:8182/iiif/2/eddie.jpg/info.json` 

If you install the current (4.1.2) version of Cantaloupe:

* Any URIs that depend on a hard-coded `Cataloupe-3.4.2` path step need to be changed to `cantaloupe-4.1.2` (note the case change if you’re on a case-sensitive file system).
* The line numbers to edit the Cantaloupe configuration files are slightly off, and the property names have also changed slightly: the values you want are (beginning on line 104) are:

```
# Enables the Control Panel, at /admin.
endpoint.admin.enabled = true
endpoint.admin.username = admin
endpoint.admin.secret = yolo
```

* To configure the image directory under 4.1.2, in the admin dashboard click on *Source* and then *FilesystemSource*. 

### The presentation API

* Outline: <https://iiif.github.io/training/iiif-5-day-workshop/day-two/0-presentation-api-introduction.html>
* Slides: <https://docs.google.com/presentation/d/1yi4x3Vpx00jx8SijcTt63BBHufpHfRf2O3oszDPH5KQ/edit#slide=id.g4bf2f5b8f7_0_156>

The presentation API serves *presentation semantic metadata*, rather than *descriptive semantic metadata*. That is, not about how to catalog images. Instead, URI-based information for search, display, reuse; about structure and how to navigate; about pagination, orientation, reading direction, and other hints about *presentation* (including popup annotation on regions, where the presentation API has to store the location on the image and the content of the annotation). The presentation API *drives the viewing experience*, that is, guides what the image server provides to the image viewer. Con venient for *compound* or *complex* objects, e.g., fragments. Documentary metadata also comes from the presentation API.

Manifest (typically, although not obligatorily, a single file) contains enough information (including contextual information) to display content to user. includes ordering, structure (e.g., table of contents) and descriptive information for resource and individual images. Manifest refers to images using the image API, telling the viewer where to find them, and the viewer then requests them directly from the server. The manifest can link to multiple image servers, annotation servers, etc., thus unifying distributed information and data.

#### Anatomy of a basic manifest

*Manifests* (currently version 2) for *things*: objects (books, painting, manuscripts, maps, collections, etc.). Contains *presentation metadata* and, optionally, *descriptive metadata* (not the main goal). A manifest is made up of *canvases*, which have content annotated (painted) onto (sic; “annotate an image onto a canvas” is the technical terminology) them.

A canvas might hold an image, coments about sections of the image, transcriptions of textual areas of the image, etc. 

##### Basic resource types

*Annotations for content*, where content can be images, audio, video, text, links to other resources, etc. Version 2 uses Open annotation data model; Version 3 will use Web annotation data model. The image is just one of the pieces of content on the canvas, which may contain multiple images or no image at all. We think we’re annotating the image, but we’re really annotating the canvas.

*Sequences for structure* (ordering). Multiple canvases may be ordered; the manifest may contain a section of sequences. There may be multiple sequences for the same canvases, e.g., if page ordering is contested. In practice most mainfests contain a single sequence.

*Ranges for structure* (grouping). Sequence structure (not just set), allowing for representation of internal structure. Canvases may belong to more than one range. Powers TOC, e.g., chapters and sections of books or sides A and B of an LP. Implemented in viewers as a TOC panel. Listed under *structures* section of manifest. Will change in Version 3, which is more hierarchical.

Sequences and ranges are both ordered. Sequences are intended to be exhaustive (every canvas), while ranges are typically selective (e.g., table of contents).

*Collections* for high-level structures, which have their own pseudo-manifests, which point to other manifests.

Manifest uses JSON-LD, which differs slightly from standard JSON. 

Canvas is central to the model. Multiple images may be layered onto an object, and the viewer can provide a choice.

#### Building a group manifest

Bodleian Digital Manuscripts Toolkit <http://dmt.bodleian.ox.ac.uk>, featuring the Bodleian Manifest Editor at <http://dmt.bodleian.ox.ac.uk/manifest-editor/>. In Real Life, manifests are typically automatically generated from images or catalog records. Other tools:

* https://github.com/edsilv/biiif (command line)
* https://github.com/iiif-prezi/osullivan (Ruby library)
* https://github.com/iiif-prezi/iiif-prezi (Python tools)

Under Images,

```
"motivation": "sc:painting",
"on": "https://example.com/my-canvas/1",
```

means that the purpose is to paint the image onto the canvas. It does not mean that the image is a painting.

The "profile" (under "service") is copied from *image.json*, and other properties may be, as well (it helps the viewer, but is optional). Level 1 is zoomable, level 0 is static, level 2 is the most interactive. The @id under "service" has to be a reachable URL; it isn’t just a URI. The spec explains when @id values must be resolvable.

Validate manifest locally with <https://github.com/DDMAL/tripoli>.

#### Building an individual manifest

Create a new manifest in the Bodleian editor, locally or remotely. Run in Firefox; Chrome blocks remote resources.

#### Serving your manifest

<https://iiif.github.io/training/iiif-5-day-workshop/day-two/4-serving-your-manifest.html>

Manifests need to live on the web, and images need to be served by IIIF servers on the web. Start with:

`http-server -p 1234 --cors`

where the value of the `-p` parameter is the port.

#### Viewing your manifest

### Building a simple gallery viewer

<https://iiif.github.io/training/iiif-5-day-workshop/day-two/6-building-a-gallery-viewer-intro.html>

Plain ol’ web page, with `<img>` elements that point to IIIF images. To find some images, see <https://searchworks.stanford.edu/catalog?f%5Biiif_resources%5D%5B%5D=available>; click on the Network button to drill down to the manifest and drill down to the image URI there. Kinda laborious, no metadata, not very functional. The point of the exercise is that this *isn’t* the way to build an image gallery.

## 2019-06-05

### Annotations

* GitBook chapter: <https://iiif.github.io/training/iiif-5-day-workshop/day-three/annotations-and-annotation-lists.html>
* Slides: <https://slides.com/hadro/dhsi-workshop-day-3-annotations>

Almost everything is an *annotation*. Spacially, a *manifest* is hierarchical: *collection* → *manifest* → *sequence* → *canvas* → *content*. The content is *painted onto* the canvas. Multiple canvases can be assembled and ordered in a sequence. The manifest pulls together the canvases and their content in an organized way.

The most important properties are the `@id` values of the manifest, the canvas, and the image resource.

In a manifest, the manifest-level key:value pairs are:

* *@context* specifies the presentation API version and thus declares the available properties. The value must resolve to the appropriate JSON file; it is not intended for human consumption.
* *@type* declares it to be a manifest, using a namespaced datatype: `sc:Manifest`
* *@id* is a URI that serves as a unique identifier. It should resolve, and if you move the manifest, you should change the value. (Hmm: the Bodleian manifest editor created the following key:value pair for a manifest: `"@id": "http://9919cd38-9b13-4290-9fe4-d31da31f6cd8"`.) It’s intended as a subject for a LOD triple.
* *label* is the primary title
* *description* is more extended, and may include multiple languages as an array of JSON objects that specify `@value` and `@language`
* *attribution* may be a string identifying the holding institution, but there isn’t always one (e.g., the Internet Archive, where it’s the *contributing* institution). 
* *logo* is a URL that points to a graphic logo
* *sequences* is an array of JSON objects. Typically there is one sequence, but there may be more than one.

A sequence may contain the following key:value pairs:

* *@type* is `sc:Sequence`. `@context` is inherited from the top (manifest) level unless overwritten.
* *canvases* is an array of canvases

A canvas has the following key:value pairs:

* *@type* must be `sc:Canvas`
* *@id* must be unique and doesn’t have to resolve, but it must be unique. (If it isn’t, it uses only the last value, but this is an error, and not a feature.) Annotations will link to a canvas @id, which should be persistent.
* *label* is the title for the canvas
* *width* and *height* of the canvas; not necessarily the width and height of the image painted (annotated) onto the canvas. If the image is larger than the canvas, it will be truncated.
* *images* has header information:
	* *@type* has the value `oa:Annotation` (it is annotated onto the canvas)
	* *motivation* has the value `sc:painting` (the way it is annotated is that it is *painted* onto the canvas)
	* *on*: the `@id` of the canvas onto which it’s painted. Should be there, and must be there if you aren’t painting onto the entire canvas, but the Bodleian editor doesn’t create it.

*images* contains *resource*, which has:

* *@id* must resolve to IIIF URI, e.g., "http://obdurodon.org:8182/iiif/2/obdurodon.jpg/full/full/0/default.jpg" (size, fragment, etc. may differ)
* *@type* is `dctypes:Image`
* *format*, e.g., `image/jpeg` (optional)
* *service*, header information for the image itself, which is required for IIIF viewers, and is not implemented in all of them:
	* *@context* pointer to the image API (NB: not the same as manifest @context), specifies what you can ask of or do with the resource. Specifies propertiers and element for JSON-LD. e.g., "http://iiif.io/api/image/2/context.json"
	* *@id* points to base of image, e.g., "http://obdurodon.org:8182/iiif/2/obdurodon.jpg"
	* *profile*: for the viewer, e.g., "http://iiif.io/api/image/2/level2.json"

### Demos

* Grandes Chroniques de France - Châteauroux BM ms. 5 
Démo de reconstitution virtuelle du manuscrit 5 de Châteauroux, <https://demos.biblissima.fr/chateauroux/> reunifies disaggregated manuscript. 
* Compariscopoe puts multiple images on a canvas. Use <https://vanda.github.io/iiif-features/compariscope.html> with any image resource. Changes the opacity of multiple images in sync and makes it possible to drag and drop them. The canvas has three images painted onto it; the tool is JavaScript that lets you manipulate each of the three separately.
* States of Mind exhibit at <http://ghp.wellcomecollection.org/annotation-viewer/quilt/#0> layers one image annotation and multiple geometric annotations that describe the pieces. Selecting a geometric annotation zooms into the fragment.
* The Butler-Bowdon Cope <https://www.vam.ac.uk/articles/the-butler-bowdon-cope>. Click on a plus sign to zoom and crop (adjust the image API) and display an annotation for a particular location (paint another annotation onto the canvas). The viewer is Digirati’s Canvas Panel (<https://github.com/digirati-co-uk/canvas-panel>). *Guided viewing*. A newer version can use a timer to walk the reader through the views.
* Ten Thousand Rooms (<https://tenthousandrooms.yale.edu/node/106/mirador?canvas=2946>) allows uploads and provides tools to create annotations, served inside a modified version of Mirador. The popup annotations are clickable and display metadata in a sidebar. There’s a dropdown at the top of the annotation panel to toggle among transcription, translation, and other features. You can open multiple annotation sidebars at once.

### Theory

We can paint text onto a canvas the same way we can paint images, and the painting can be onto just a zone on the canvas. And we aren’t limited to just image and text, e.g., the Chopin nocture annotations we saw earlier. See now <https://ddmal.github.io/IIIF-AV-player> (proof of concept, and not entirely valid, since it was completed before 3.0 of the presentation API was finalized). The video is drawn from YouTube (it’s the underlying H.264 stream, not the YouTube player); the highlighting was brute-forced.

To paint onto a *fragment* (an area of a canvas), set "on" to something like "https://bvmm.irht.cnrs.fr/iiif/2309/canvas/canvas-981384#xywh=3949,994,1091,1232". `xywh` is simplest, but other shapes are possible. See the Gallica examples at <https://gist.githubusercontent.com/hadro/44b893d9be9ac31e705f1c034ae61d84/raw/0dd43c7570c3f30cf37385207524304f785a196d/gallica.json>.

Customized version of Mirador: <https://iiif.github.io/training/iiif-5-day-workshop/day-three/mirador.html> that can render customized fragment of manifest. In Mirador, multiple images on a canvas are thought of as layers, and not all layers are rendered automatically. Open side panel and click on tabTitleLayers. The order of annotations matters; do the larger one first.

### Annotation example

* Item: <https://wellcomelibrary.org/moh/report/b18250464>
* Manifest:
<https://wellcomelibrary.org/iiif/b18250464/manifest>

Search within the manifest for "otherContent". What do you find there? Where does it fit within the manifest hierarchy?

What you find under "otherContent" is a list of one object with three key:value pairs: @id, @type, and label. The @type value is "sc:annotationList". The @id is an identifier, which has to resolve because it’s where the content is going to be retrieved. 

The annotation list is a JSON file that uses the presentation API as its @context, with @type "sc:annotationList". The resources paint snippets of text onto location on the image. The value of the "on" property is vital.

An annotation list must exist as a separate file; it cannot be embedded in the manifest. Embedding will become possible in version 3. 

IIIF annotation building tool: <https://ncsu-libraries.github.io/iiif-annotation/>.

### Annotation in Mirador

To specify a port for the simple annotation server, use `java -jar dependency/jetty-runner.jar --port 9090 simpleAnnotationStore.war`

Transkribus (<https://transkribus.eu/Transkribus/>) will identify coordinates for lines. Not IIIF, but can be exported and reformatted.
`