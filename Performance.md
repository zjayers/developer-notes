# __Performance__

- [__Performance__](#performance)
- [***Front End***](#front-end)
  - [***Critical Render Path***](#critical-render-path)
  - [***Optimizing The Critical Render Path***](#optimizing-the-critical-render-path)
  - [HTML](#html)
  - [CSS](#css)
  - [Javascript](#javascript)
  - [Code Optimization](#code-optimization)
    - [Tree Shaking](#tree-shaking)
    - [React Component Performance](#react-component-performance)
    - [Code Performance Checking](#code-performance-checking)
    - [Code Splitting](#code-splitting)
      - [Component Based Chunking With React](#component-based-chunking-with-react)
      - [Route Based Chunking With React](#route-based-chunking-with-react)
        - [Method 1 - Manual Code Splitting](#method-1---manual-code-splitting)
        - [Method 2 - Code Splitting with Async Components](#method-2---code-splitting-with-async-components)
        - [Method 3 - React.lazy()](#method-3---reactlazy)
    - [Progressive Web Apps](#progressive-web-apps)
      - [HTTPS](#https)
      - [App Manifest](#app-manifest)
      - [Service Worker](#service-worker)
      - [Deploying](#deploying)
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

## Code Optimization

  - ***Fast Time to first meaningful paint***

  - ***Fast Time to page becoming interactive***

  - ***Only load what's needed***

  - ***Avoid Blocking Main Thread***

  - ***Avoid Memory Leaks***

  - ***Avoid Needless Re-Rendering***

### Tree Shaking

  - https://developers.google.com/web/fundamentals/performance/optimizing-javascript/tree-shaking/

### React Component Performance

  - https://github.com/maicki/why-did-you-update
  - Keep components from updating needlessly

  ```JSX
  shouldComponentUpdate(nextProps, nextState) {
    if (this.state.<var> !== nextState.<var>) {
      return true;
    }
    return false;
  }
  ```

### Code Performance Checking
  ***Chrome Dev Tools***
  - ***React Performance*** - add query string  `?react_perf`  to address
  - Record and scrub through page contents
  - Summary - Bottom Up - Call Tree - Event Log
    - All used to show the website loading performance

  ***Javascript Execution***

    Request JS => JS Arrives => fetch() => Content Arrives => Page Is Now Interactive

  ***Page Testing Tools***
  - *webpagetest.org*

### Code Splitting

- ***Javascript benefits from being in small chunks, to avoid blocking the main thread***
- ***Code is split between the pages they are meant for - rather than one large bundle file***

  - Vendor File - always gets loaded *exp: react library*

  - other files can be 'lazy-loaded' after the main file is loaded


#### Component Based Chunking With React

- https://github.com/jamiebuilds/react-loadable
- https://reactjs.org/docs/code-splitting.html

#### Route Based Chunking With React

##### Method 1 - Manual Code Splitting
  - Use import statements inside of code blocks

  ```JSX
  import React, { Component } from 'react';
  import './App.css';

  import Page1 from './Components/Page1';
  import Page2 from './Components/Page2';
  import Page3 from './Components/Page3';

  class App extends Component {

    constructor() {
      super();
      this.state = {
        route: 'page1',
        component: null
      }
    }

    onRouteChange = (route) => {
        //Code Splitting - manual
        if (route === 'page1') {
          this.setState({ route: route })
        } else if (route === 'page2') {
          import('./Components/Page2')
            .then((Page2) => {
              this.setState({ route: route, component: Page2.default })
            })
            .catch(err => {
            });
        } else {
          import('./Components/Page3')
            .then((Page3) => {
              this.setState({ route: route, component: Page3.default })
            })
            .catch(err => {
              console.log(err)
            });
        }
    }

    render() {
        Part 2 - No Code Splitting - manual
        if (this.state.route === 'page1') {
          return <Page1 onRouteChange={this.onRouteChange} />
        } else {
          return <this.state.component onRouteChange={this.onRouteChange} />
        }
    }
  }

  export default App;
  ```
---
##### Method 2 - Code Splitting with Async Components
  - Use an asynchronous component to wrap components

  ```JSX
  import React, { Component } from 'react';
  import './App.css';

  import Page1 from './Components/Page1';
  import AsyncComponent from './AsyncComponent';

  class App extends Component {

    constructor() {
      super();
      this.state = {
        route: 'page1',
      }
    }

    onRouteChange = (route) => {
      this.setState({ route: route });
    }

    render() {
        Part 3 - Cleaner Code Splitting
        if (this.state.route === 'page1') {
          return <Page1 onRouteChange={this.onRouteChange} />
        } else if (this.state.route === 'page2') {
          const AsyncPage2 = AsyncComponent(() => import("./Components/Page2"));
          return <AsyncPage2 onRouteChange={this.onRouteChange} />
        } else {
          const AsyncPage3 = AsyncComponent(() => import("./Components/Page3"));
          return <AsyncPage3 onRouteChange={this.onRouteChange} />
        }
    }
  }

  export default App;
  ```

  ```JSX
  import React, { Component } from "react";

  export default function asyncComponent(importComponent) {
    class AsyncComponent extends Component {
      constructor(props) {
        super(props);
        this.state = {
          component: null
        };
      }

      async componentDidMount() {
        const { default: component } = await importComponent();

        this.setState({
          component: component
        });
      }

      render() {
        const Component = this.state.component;

        return Component ? <Component {...this.props} /> : null;
      }
    }
    return AsyncComponent;
  }
  ```
---

##### Method 3 - React.lazy()

  ```JSX
  import React, { Component, Suspense } from 'react';
  import './App.css';

  import Page1 from './Components/Page1';
  const Page2Lazy = React.lazy(() => import('./Components/Page2'));
  const Page3Lazy = React.lazy(() => import('./Components/Page3'));

  class App extends Component {
    constructor() {
      super();
      this.state = {
        route: 'page1',
      }
    }
    onRouteChange = (route) => {
      this.setState({ route: route });
    }
    render() {
      if (this.state.route === 'page1') {
        return <Page1 onRouteChange={this.onRouteChange} />
      } else if (this.state.route === 'page2') {
        return (
          <Suspense fallback={<div>Loading...</div>}>
            <Page2Lazy onRouteChange={this.onRouteChange} />
          </Suspense>
        );
      } else {
        return (
          <Suspense fallback={<div>Loading...</div>}>
            <Page3Lazy onRouteChange={this.onRouteChange} />
          </Suspense>
        );
      }
    }
  }

  export default App;
  ```

### Progressive Web Apps

  - Works like a native (mobile) app on a browser
  - Available Offline - Has access to hardware
  - https://developers.google.com/web/progressive-web-apps/checklist

#### HTTPS
  - PWAs must use HTTPs to work properly
  - https://letsencrypt.org/
  - https://www.cloudflare.com/


#### App Manifest
  - Mimic the native shell
  - `create-react-app` creates a `manifest.json` file
  - Generate favicons for app
  - https://realfavicongenerator.net/

#### Service Worker
  - update `serviceWorker.unregister()` to `serviceWorker.register()` in `index.js`
  - *Service worker is currently disabled in developer tools*
  - Script that browser runs in the background
  - Intercepts any requests to the network and determines if the network actually needs to be accessed
  - Workers have access to the cache API
  - Used for features that don't need interaction
  - Works as a programmable proxy
  - This is the reason progressive web apps can work offline
  - https://jakearchibald.github.io/isserviceworkerready/

#### Deploying
  - install npm package `gh-pages` as dependency

  - add deploy scripts to `package.json`

    ```json
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
    ```

  - add homepage link to `package.json`

    ```json
    "homepage": "https://<username>.github.io/<repo name>
    ```

  - run command `npm run deploy`

  - Go to github.com - navigate to repo - navigate to settings

  - Enable github pages and select the `gh-pages` branch


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