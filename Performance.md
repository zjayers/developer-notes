# __Performance__

- [__Performance__](#performance)
- [***Front End***](#front-end)
  - [***Critical Render Path***](#critical-render-path)
  - [***Optimizing The Critical Render Path***](#optimizing-the-critical-render-path)
  - [HTML](#html)
  - [CSS](#css)
  - [Javascript](#javascript)
- [***Back End***](#back-end)
  - [CDNs](#cdns)
  - [Caching](#caching)
  - [Load Balancing](#load-balancing)
  - [DB Scaling](#db-scaling)
  - [GZIP](#gzip)
- [***Network Performance***](#network-performance)
  - [HTTP/2](#http2)
  - [Minimize HTML/CSS/JS](#minimize-htmlcssjs)
  - [Minimize Images](#minimize-images)
      - [General Rules](#general-rules)
      - [Best Image Types](#best-image-types)
  - [Compression Helpers](#compression-helpers)
  - [Media Queries](#media-queries)
      - [Media Query Cheat Sheet](#media-query-cheat-sheet)
- [***Resources: Performance Tools***](#resources-performance-tools)
  - [Other Resources:](#other-resources)
  - [Additional image manipulation tools:](#additional-image-manipulation-tools)

# ***Front End***

## ***Critical Render Path***

***DOM => CSSOM => Content Loaded => Render Tree => Layout => Paint => Load Event Occurs***

- Browser Combines The HTML-DOM & CSSOM & Javascript Files to create a render tree
- The Render Tree is used to construct the page layout

## ***Optimizing The Critical Render Path***

##  HTML
- Download CSS files as soon as possible
  - Place CSS files in the ``<HEAD>`` tag
- Download JS files as late as possible
  - Place JS files at the bottom of the ``<BODY>`` tag

## CSS
- Only load whatever is needed
- Above the fold loading
  ```html
  <!-- script to load non-important CSS styles after page load -->
  <script type='text/javascript'>

    const loadStyleSheet = src => {

      if (document.createStylesheet) {

        document.createStylesheet(src)

      } else {

        const stylesheet = document.createElement('link');
        stylesheet.href = src;
        stylesheet.type='text/css';
        stylesheet.rel = 'stylesheet';
        document.getElementsByTagName('head')[0].appendChild('stylesheet');

      }
    }

    window.onload = function() {

      loadStyleSheet('./belowTheFold.css');

    }

  </script>
  ```
- Add Media Attributes (Queries)

  - [Media Query Cheat Sheet](#media-query-cheat-sheet)

- Use Less Specificity
  ```css
  /* Bad */
  .header .nav .item .link a.important {
    ...
  }

  /* Good */
  a.important {
    ...
  }
  ```

## Javascript

- https://stackoverflow.com/questions/10808109/script-tag-async-defer
- Load Scripts Asynchronously
  ```html
  <!-- Load pages asynchronously
  - Add to anything that doesnt affect the DOM or CSSOM
  - Google Analytic Scripts - Tracking Scripts -->

  <script async>
  ```
- Defer Loading Of Scripts
  ```html
  <!-- Defer Loading
  - If core functionality doest not require scripts - use defer
  - Third party scripts that are not above the fold-->

  <script defer>
  ```

- Avoid Long Running Javascript Files
-

# ***Back End***

## CDNs

## Caching

## Load Balancing

## DB Scaling

## GZIP

# ***Network Performance***

## HTTP/2
- https://developers.google.com/web/fundamentals/performance/http2/

## Minimize HTML/CSS/JS
- Webpack Used During Build Process

## Minimize Images

#### General Rules
-  __Resize images based on size to be displayed__

- __Use media queries to optimize image sizes for screen size__

- __Remove Image Metadata__

#### Best Image Types

- JPG *(No Transparency)*
- GIF *(Reduces color count for file saving)*
- PNG *(Limits number of color - smaller than JPGs - Have Transparency)*
- SVG *(Vector Graphics)*

##  Compression Helpers
- PNG - __*[TinyPNG.com](https://tinypng.com/)*__
- JPG - __*[JPEG-optimizer.com](http://jpeg-optimizer.com/)*__
  - *Always lower JPG image quality (30% - 60%)*
- CDNs - __*[imigx.com](https://www.imgix.com/)*__
- MetaData - __*[verexif.com](http://www.verexif.com/)*__

## Media Queries

#### Media Query Cheat Sheet

```css
/*------------------------------------------
  Responsive Grid Media Queries - 1280, 1024, 768, 480
   1280-1024   - desktop (default grid)
   1024-768    - tablet landscape
   768-480     - tablet
   480-less    - phone landscape & smaller
--------------------------------------------*/
@media all and (min-width: 1024px) and (max-width: 1280px) { }

@media all and (min-width: 768px) and (max-width: 1024px) { }

@media all and (min-width: 480px) and (max-width: 768px) { }

@media all and (max-width: 480px) { }

/*------------------------------------------
  Foundation Media Queries
   http://foundation.zurb.com/docs/media-queries.html
--------------------------------------------*/

/* Small screens - MOBILE */
@media only screen { } /* Define mobile styles - Mobile First */

@media only screen and (max-width: 40em) { } /* max-width 640px, mobile-only styles, use when QAing mobile issues */

/* Medium screens - TABLET */
@media only screen and (min-width: 40.063em) { } /* min-width 641px, medium screens */

@media only screen and (min-width: 40.063em) and (max-width: 64em) { } /* min-width 641px and max-width 1024px, use when QAing tablet-only issues */

/* Large screens - DESKTOP */
@media only screen and (min-width: 64.063em) { } /* min-width 1025px, large screens */

@media only screen and (min-width: 64.063em) and (max-width: 90em) { } /* min-width 1024px and max-width 1440px, use when QAing large screen-only issues */

/* XLarge screens */
@media only screen and (min-width: 90.063em) { } /* min-width 1441px, xlarge screens */

@media only screen and (min-width: 90.063em) and (max-width: 120em) { } /* min-width 1441px and max-width 1920px, use when QAing xlarge screen-only issues */

/* XXLarge screens */
@media only screen and (min-width: 120.063em) { } /* min-width 1921px, xlarge screens */

/*------------------------------------------*/



/* Portrait */
@media screen and (orientation:portrait) { /* Portrait styles here */ }
/* Landscape */
@media screen and (orientation:landscape) { /* Landscape styles here */ }


/* CSS for iPhone, iPad, and Retina Displays */

/* Non-Retina */
@media screen and (-webkit-max-device-pixel-ratio: 1) {
}

/* Retina */
@media only screen and (-webkit-min-device-pixel-ratio: 1.5),
only screen and (-o-min-device-pixel-ratio: 3/2),
only screen and (min--moz-device-pixel-ratio: 1.5),
only screen and (min-device-pixel-ratio: 1.5) {
}

/* iPhone Portrait */
@media screen and (max-device-width: 480px) and (orientation:portrait) {
}

/* iPhone Landscape */
@media screen and (max-device-width: 480px) and (orientation:landscape) {
}

/* iPad Portrait */
@media screen and (min-device-width: 481px) and (orientation:portrait) {
}

/* iPad Landscape */
@media screen and (min-device-width: 481px) and (orientation:landscape) {
}

<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
```

# ***Resources: Performance Tools***

- View main thread activities in a table to sort activities based on which ones took up the most time.

- Analyze frames per second (FPS) to measure whether your animations truly run smoothly.

- Monitor CPU usage, JS heap size, DOM nodes, layouts per second, and more in real-time with the Performance Monitor.

- Capture screenshots while recording to play back exactly how the page looked while the page loaded, or an animation fired, and so on.

- View interactions to quickly identify what happened on a page after a user interacted with it.

- Find scroll performance issues in real-time by highlighting the page whenever a potentially problematic listener fires.

- View paint events in real-time to identify costly paint events that may be harming the performance of your animations.

- View main thread activity to view every event that occurred on the main thread while you were recording.

## Other Resources:

- http://optimizilla.com/

- https://tools.pingdom.com/

- https://www.thinkwithgoogle.com/feature/mobile/

- https://developers.google.com/web/tools/lighthouse/

- http://websitespeedranker.com/

- https://pageweight.imgix.com/

- https://developers.google.com/speed/pagespeed/insights/

- https://passmarked.com/

- https://images.guide/

- https://www.crazyegg.com/blog/image-editing-tools/



## Additional image manipulation tools:

- **XNConvert**: This free, cross-platform tool can handle batched images, and performs resizing, optimization, and other transforms.

- **ImageOptim**: This free tool is available for Mac and as an online service, and is specifically aimed at optimizing images for speed, including metadata removal (discussed above).

- **ResizeIt**: A Mac-only desktop product that lets you change the size of multiple images simultaneously, and can convert file formats at the same time.

- **PicResize**: One of several good browser-based tools that gives you lots of options for cropping, rotating, resizing, adding effects to, and converting images.

- **Gimp**: This ever-popular cross-platform tool just gets better with age. Powerful and flexible, Gimp lets you perform a wide variety of image manipulation tasks including, of course, resizing.