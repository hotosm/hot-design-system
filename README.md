# HOT Design System

**An early attempt to start a HOT Design System.** Forked from the `oam-design-system`. Purpose is meant to follow the `oam-design-system` UI style but brand specifically for the HOT logo and colors.

The following guide only explains how to include the `hot-design-system` in a new project. For usage information check the [documentation website](http://hotosm.github.io/hot-docs/).  

For information on how to develop the `hot-design-system` checkout the [DEVELOPMENT.md](DEVELOPMENT.md)  

---

Style guide and UI components library that aims to standardize the look and feel across all hot-related applications, while defining coding best practices and conventions.

Install it as an `npm` module: (module not available yet. use direct link)
```
npm install https://github.com/hotosm/hot-design-system#v1.0.0
```
For the most recent version omit the tag.

**Note:**
This design system makes some assumptions which are described below for each of the elements.  
Check the build system of [oam docs](https://github.com/hotosm/oam-docs/blob/master/gulpfile.js), a project that uses the `oam-design-system`.

## Overview

The shared assets are all in the `assets` directory. It is organized as follows:

### assets/scripts
Utility libraries and shared components.

**USAGE**  
Use as any node module:
```js
import { Dropdown, user } from 'hot-design-system';
```
If you want to minimize bundle size you can also include the components directly.  
Bindings exported from `hot-design-system` are also available in `hot-design-system/assets/scripts`

### assets/styles
Requires [Bourbon](https://github.com/lacroixdesign/node-bourbon) and [Jeet](https://github.com/mojotech/jeet).

**INSTALLATION**  
Add the module path to the `includePaths` of gulp-sass. Should look something like:
```js
.pipe($.sass({
  outputStyle: 'expanded',
  precision: 10,
  includePaths: require('node-bourbon').with('node_modules/jeet/scss', require('hot-design-system/gulp-addons').scssPath)
}))
```

The `hot-design-system` uses **Open Sans** which is available on [Google Fonts](https://goo.gl/FZ0Ave).  
It needs to be included in the app:
```
<link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Open+Sans:300italic,400italic,700italic,400,300,700" />
```

**USAGE**  
Now you can include it in the main scss file:
```scss
// Bourbon is a dependency
@import "bourbon";

@import "jeet/index";

@import "hot-design-system";
```

### assets/icons
The `hot-design-system` includes svg icons that are compiled into a webfont and included in the styles.  
To use them check the `_hot-ds-icons.scss` for the class names.

### assets/graphics
Graphics that are to be shared among projects.

**INSTALLATION**  
Add the `graphicsMiddleware` to browserSync. This is only to aid development.  
Should look something like:
```js
browserSync({
  port: 3000,
  server: {
    baseDir: ['.tmp', 'app'],
    routes: {
      '/node_modules': './node_modules'
    },
    middleware: require('hot-design-system/gulp-addons').graphicsMiddleware(fs) // <<< This line
  }
});
```
*Basically every time there's a request to a path like `/assets/graphics/**`, browserSync will check in the `hot-design-system` folder first. If it doesn't find anything it will look in the normal project's asset folder.*

You also need to ensure that the images are copied over on build.
This ensures that the graphics are copied over when building the project.
```js
gulp.task('images', function () {
  return gulp.src(['app/assets/graphics/**/*', require('hot-design-system/gulp-addons').graphicsPath + '/**/*'])
    .pipe($.cache($.imagemin({
```

**USAGE**  
Just include the images using the path `assets/graphics/[graphic-type]/[graphic-name]`.  
All available images can be found [here](assets/graphics/).

## License
HOT Design System is licensed under **BSD 3-Clause License**, see the [LICENSE](LICENSE) file for more details.
