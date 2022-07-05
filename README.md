[![pdiiif logo](pdiiif-web/assets/logo.svg)](https://pdiiif.jbaiter.de)

[**Demo**](https://pdiiif.jbaiter.de)

[**Sample PDF generated with the library**](https://pdiiif.jbaiter.de/wunder.pdf)

[**Library API Documentation**](https://jbaiter.github.io/pdiiif)

**pdiiif** is a JavaScript library to **create PDFs from IIIF Manifests.**
For the most part, it runs both in browsers (that implement the
[File System Access API](https://caniuse.com/native-filesystem-api),
currently Chrome/Chromium Desktop and Edge only) and as a Node.js server-side
application. When generating a PDF in the browser, almost all communication happens
directly between the user's browser and the IIIF APIs referenced from the Manifest.
The only exception is for generating the cover page, which by default needs to be
generated on the server.

It comes with a small **sample web application** that demonstrates
how to use the library in the browser, you can check out a public instance
of it on https://pdiiif.jbaiter.de, the source code is contained in the
[`pdiiif-web` subdirectory](https://github.com/jbaiter/pdiiif/tree/main/pdiiif-web).

A main goal of the **library** is to be as _memory-efficient_ as possible, by
never holding more than a few pages in memory and streaming directly to
the user's disk (via chunked-encoding in the HTTP response on the server,
and by making use of the new Native Filesystem API in browsers).

It is also well-suited for embedding in other applications due to
its relatively small footprint, the example web application comes in at
**~116KiB gzipped** with all dependencies (if you use `manifesto.js` from
the IIIF commons in your application already, the total footprint will be
~30% smaller).

In addition to the images on the IIIF Canvases referenced in the manifest,
the library can create a **hidden text layer** from OCR associated with
each canvas (ALTO or hOCR referenced from a canvas' `seeAlso` property).

**Features**

- [x] PDF Page for every single-image Canvas in a Manifest
- [x] Rendering Canvases with multiple images
- [x] PDF Table of Contents from IIIF Ranges
- [x] Cover page with metadata, attribution and licensing information
- [x] Hidden text layer from ALTO or hOCR OCR
- [ ] Optional rendering of IIIF Annotations as PDF annotations _(planned for v0.2, Q3/4 2022)_
- [ ] Extraction of PDF annotations as IIIF anotations from PDFs generated with pdiiif _(planned for v0.2, Q3/4 2022)_

**Cookbook Matrix**

The [IIIF Cookbook](https://iiif.io/api/cookbook/) has a matrix of "recipes" with viewer support, here's an overview
of the recipe support in pdiiif:

<details>
<summary><strong>Basic Recipes</strong> (4 of 6 supported)</summary>

- [x] [Simplest Manifest - Single Image File](https://iiif.io/api/cookbook/recipe/0001-mvm-image/): Partial, only for JPEG images, **Cookbook example doesn't work** due to use of PNG
- [ ] [Simplest Manifest - Audio](https://iiif.io/api/cookbook/recipe/0002-mvm-audio/): NO, PDF has support for audio, but support in pdiiif unlikely, unless there is substantial demand for it
- [ ] [Simplest Manifest - Video](https://iiif.io/api/cookbook/recipe/0003-mvm-video/): NO, PDF has support for video, but support in pdiiif unlikely, unless there is substantial demand for it
- [x] [Support Deep Viewing with Basic Use of a IIIF Image Service](https://iiif.io/api/cookbook/recipe/0005-image-service/): YES, Deep Viewing isn't useful in PDF, but IIIF Image Services are fully supported
- [x] [Internationalization and Multi-language Values (label, summary, metadata, requiredStatement)](https://iiif.io/api/cookbook/recipe/0006-text-language/): YES
- [x] [Simple Manifest - Book](https://iiif.io/api/cookbook/recipe/0009-book-1/): YES
</details>

<details>
<summary><strong>IIIF Properties</strong> (6 of 15 supported)</summary>

- [x] [Embedding HTML in descriptive properties (label, summary, metadata, requiredStatement)](https://iiif.io/api/cookbook/recipe/0007-string-formats/): Partially, only for server-generated cover page
- [x] [Rights statement (rights, requiredStatement)](https://iiif.io/api/cookbook/recipe/0008-rights/): YES
- [ ] [Viewing direction and Its Effect on Navigation (viewingDirection)](https://iiif.io/api/cookbook/recipe/0010-book-2-viewing-direction/): NO
- [ ] [Book 'behavior' Variations (continuous, individuals) (behaviorimage)](https://iiif.io/api/cookbook/recipe/0011-book-3-behavior/): NO, support unlikely since paging preference is global in PDF
- [ ] [Load a Preview Image Before the Main Content (placeholderCanvas)](https://iiif.io/api/cookbook/recipe/0013-placeholderCanvas/): NO, not applicable
- [ ] [Audio Presentation with Accompanying Image (accompanyingCanvas)](https://iiif.io/api/cookbook/recipe/0014-accompanyingcanvas/): NO, no support for audio
- [ ] [Begin playback at a specific point - Time-based media (start)](https://iiif.io/api/cookbook/recipe/0015-start/): NO, no support for time-based media
- [x] [Metadata on any Resource (metadata)](https://iiif.io/api/cookbook/recipe/0029-metadata-anywhere/): Partial, only Manifest metadata
- [ ] [Providing Alternative Representations (rendering)](https://iiif.io/api/cookbook/recipe/0046-rendering/): NO, but support possible via PDF layers
- [ ] [Linking to Structured Metadata (seeAlso)](https://iiif.io/api/cookbook/recipe/0053-seeAlso/): NO, could be placed on the cover page
- [x] [Image Thumbnail for Manifest (thumbnail)](https://iiif.io/api/cookbook/recipe/0117-add-image-thumbnail/): YES
- [x] [Displaying Multiple Values with Language Maps (label, summary, metadata, requiredStatement)](https://iiif.io/api/cookbook/recipe/0118_multivalue/): YES
- [ ] [Load Manifest Beginning with a Specific Canvas (start)](https://iiif.io/api/cookbook/recipe/0202-start-canvas/): NO, but support possible
- [ ] [Navigation by Chronology (navDate)](https://iiif.io/api/cookbook/recipe/0230-navdate/): NO
- [x] [Acknowledge Content Contributors (provider)](https://iiif.io/api/cookbook/recipe/0234-provider/): YES
</details>

<details>
<summary><strong>Structuring Resources</strong> (2 of 6 supported)</summary>

- [x] [Table of Contents for Book Chapters (structures)](https://iiif.io/api/cookbook/recipe/0024-book-4-toc/): YES
- [ ] [Table of Contents for A/V Content](https://iiif.io/api/cookbook/recipe/0026-toc-opera/): NO
- [ ] [Multi-volume Work with Individually-bound Volumes](https://iiif.io/api/cookbook/recipe/0030-multi-volume/): NO
- [ ] [Multiple Choice of Images in a Single View (Canvas)](https://iiif.io/api/cookbook/recipe/0033-choice/): NO, but support possible via PDF layers
- [ ] [Foldouts, Flaps, and Maps (behavior)](https://iiif.io/api/cookbook/recipe/0035-foldouts/): NO, support unlikely due to global paging preference in PDF
- [x] [Composition from Multiple Images](https://iiif.io/api/cookbook/recipe/0036-composition-from-multiple-images/): Partial, as long as all images have a JPEG representation
</details>

<details>
<summary><strong>Image Recipes</strong> (4 of 6 supported)</summary>

- [x] [Simplest Manifest - Single Image File](https://iiif.io/api/cookbook/recipe/0001-mvm-image/): Partial, only for JPEG images, **Cookbook example doesn't work** due to use of PNG
- [x] [Image and Canvas with Differing Dimensions](https://iiif.io/api/cookbook/recipe/0004-canvas-size/): YES
- [x] [Support Deep Viewing with Basic Use of a IIIF Image Service](https://iiif.io/api/cookbook/recipe/0005-image-service/): YES, Deep Viewing isn't useful in PDF, but IIIF Image Services are fully supported
- [x] [Simple Manifest - Book](https://iiif.io/api/cookbook/recipe/0009-book-1/): YES
- [ ] [Viewing direction and Its Effect on Navigation (viewingDirection)](https://iiif.io/api/cookbook/recipe/0010-book-2-viewing-direction/): NO
- [ ] [Load Manifest Beginning with a Specific Canvas (start)](https://iiif.io/api/cookbook/recipe/0202-start-canvas/): NO, but support possible
</details>

<details>
<summary><strong>Annotation Recipes</strong> (0 of 5 supported)</summary>

- [ ] Simple Annotation — Tagging: NO, support planned
- [ ] Tagging with an External Resource: NO, support planned
- [ ] Annotation with a Non-Rectangular Polygon: NO, support planned
- [ ] Simplest Annotation: NO, support planned
- [ ] Embedded or referenced Annotations: NO, support planned
</details>

**Quickstart**

Besides using the public instance at https://pdiiif.jbaiter.de, you can also run the app yourself.
The easiest way to do this is with Docker:

```
$ docker build . -t pdiiif
# SYS_ADMIN capabilities are required (for Puppeteer's headless Chrome instance to generate cover page PDFs)
$ docker run -p 8080:8080 --cap-add=SYS_ADMIN --name pdiiif pdiiif
```

**Structure of the repository**

- [`./pdiiif-lib`](https://github.com/jbaiter/pdiiif/tree/main/pdiiif-lib): Contains the library source code
- [`./pdiiif-api`](https://github.com/jbaiter/pdiiif/tree/main/pdiiif-api): Small node.js server application that
  is responsible for generating the cover pages and that can be used as a fallback for browsers that don't support
  the Native Filesystem API
- [`./pdiiif-web`](https://github.com/jbaiter/pdiiif/tree/main/pdiiif-web): Sample web application (using Svelte)
  to demonstrate using pdiiif in the browser
