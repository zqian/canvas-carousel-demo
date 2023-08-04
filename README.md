# canvas-carousel-demo
![image](https://github.com/tl-its-umich-edu/canvas-carousel-demo/assets/27447/741562ba-f6a3-4333-8512-b25946616684)

## Overview

This repository includes a collection of HTML, image, and JSON files that together form an announcement carousel
that can be installed or embedded on the Canvas home page (i.e. Dashboard).
The carousel makes use of Bootstrap (version 5), Bootstrap Icons, and native HTML and JavaScript functionality.

This carousel has been checked for accessibility and can be navigated with the keyboard and will announced the alt text if provided.

## Repository setup

This demo site can be forked on Github and either be left public or made private. Or you can just copy the files and host it where you like on S3 or any web host. At Michigan we just run the private repository as a Github Page the similar to the demo and make all our changes right in Github via the GUI. However we have an AWS CloudFront distribution setup as Github Pages has bandwidth limits. We typically have about 10 million requests and 500GB of traffic in a typical month and the costs total $10/month. 

If you change the name of this repository you'll have to change the value of pathStem in the html file to something else. This may be useful to be configurable in the future.

  `const pathStem = '/canvas-carousel-demo/';`

## Canvas installation

To enable the carousel, a Canvas admin must include the JavaScript below as part of a larger file uploaded in the Canvas custom theme editor.
Replace `{your domain}` with a domain that is publicly serving the `canvas_carousel_player.html` file
and the `content` and `data` directories under the `/canvas-carousel-demo/` path.
U-M ITS Teaching & Learning uses GitHub Pages to accomplish this, serving from [`tl-its-umich-edu.github.io/canvas-carousel-demo/](https://tl-its-umich-edu.github.io/canvas-carousel-demo/).

```
$(document).ready(function() {
  // Add Carousel
  $("#dashboard").before(`
    <div style="margin-bottom: 25px;">
      <iframe src='https://tl-its-umich-edu.github.io/canvas-carousel-demo/canvas_carousel_player.html' width="650" height="200" role="complementary" style="border: none;">
      </iframe>
    </div>
  `);
});
```

Once added, the carousel will launch and begin cycling slides when the Canvas home page is loaded.

## Adding slide content

To add one or more new slides, perform the following steps in order:

1) Add one or more new image files to the [`/content/` directory](/content/) in this repository.
2) For each new file, add a new object to the existing array under the `"slides"` key in the JSON file
  at [`data/slide-data.json`](/data/slide-data.json).
  Each object must contain the following keys and appropriate string values:
    - `imageFileName`: the name of an image file located in [`/content/`](/content/) in this repository
    - `linkURL`: a URL to send the user to when they select the slide
    - `altText`: alternative text for the slide image for use by assistive technologies

    A template file can be found at [`data/slide-data-template.json`](/data/slide-data-template.json).

3) Before saving or committing changes to the JSON file, check that all key-value pairs are present and the JSON is valid.

To remove slides from the carousel, remove the corresponding objects from the array.
Slides will be presented in the order they are defined in the array.
While the carousel does not programmatically limit the number of slides,
a maximum of four slides is recommended to lessen the cognitive load on users.

## Settings

The carousel also supports a single setting currently that controls whether the carousel starts on a random slide.
To change this setting, modify the value for the `"startRandom"` key in the object under the `"settings"` key in the
[`data/slide-data.json`](/data/slide-data.json) file. A `true` value will make the starting slide random,
`false` will make the starting slide be the first slide in the array of objects defined under `"slides"` key.

## Local development

Since this application is a collection of basic static files, developers can test changes using a simple server
on `localhost`. There are a number of ways to accomplish; essentially, you serve the parent directory of the
`canvas-carousel-demo` directory.

For example, you can use the following steps in order to use a simple Python Web server.

1) Navigate to the project directory.
  ```
  git clone https://github.com/tl-its-umich-edu/canvas-carousel-demo.git
  cd canvas-carousel-demo
  ```

2) Run the server using `http`, targeting the parent directory.
  ```
  python3 -m http.server --directory .
  ```

3) In your browser of choice, navigate to
[`http://localhost:8000/canvas-carousel-demo/canvas_carousel_player.html`](http://localhost:8000/canvas-carousel-demo/canvas_carousel_player.html).

## Resources

- [Canvas Theme Editor](https://community.canvaslms.com/t5/Admin-Guide/How-do-I-create-a-theme-for-an-account-using-the-Theme-Editor/ta-p/242)
- [Bootstrap Carousel](https://getbootstrap.com/docs/5.0/components/carousel/)
- [Boostrap Icons](https://icons.getbootstrap.com/)
- [Mozilla Developer Network - Element](https://developer.mozilla.org/en-US/docs/Web/API/Element)
