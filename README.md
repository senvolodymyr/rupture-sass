Rupture-sass
------------

Simple media queries for SASS (Rupture mixins for SASS)

```scss
.some-class {
  color: red;
  @include above(32em) {
    color: green;
  }
  @include tablet() {
    color: yellow;
  }
}
```

Compiles to

```css
.some-class {
  color: red;
}
@media only screen and (min-width: 32em) {
  .some-class {
    color: green;
  }
}
@media only screen and (min-width: 400px) and (max-width: 1050px) {
  .some-class {
    color: yellow;
  }
}
```

Installation
------------

`npm install rupture-sass`

Use
---

- Import package via ```@import``` directive
  ```scss
  @import 'node_modules/rupture-sass/rupture';
  // ...
  ```
- If using Webpack as css bundler
  ```scss
  @import '~/rupture-sass/rupture';
  // ...
  ```

API Documentation
-----------------

### Customization

#### Default values

```scss
$rupture: (
  mobile-cutoff: 400px,
  desktop-cutoff: 1050px,
  hd-cutoff: 1800px,
  enable-em-breakpoints: false, 
  anti-overlap: false,
  density-queries: 'dppx' 'webkit' 'moz' 'dpi',
  retina-density: 1.5,
  use-device-width: false
);
```

In order to override any value use ```$rupture: map-merge($rupture, (key: value, ..., keyN: valueN)```

```scss
// e.g.
$rupture: map-merge($rupture, (
  mobile-cutoff: 768px,
  desktop-cutoff: 1366px
));
```

##### mobile-cutoff
Upper bound where `mobile()` mixin has an effect and lower bound of the `tablet()` mixin.

##### desktop-cutoff
Lower bound where `desktop()` mixin has an effect and upper bound of the `tablet()` mixins.

##### hd-cutoff
Lower bound where `hd()` mixin starts to have an affect

##### enable-em-breakpoints
Convert pixel values into EM's values
```scss
@include below(768px) {/*...*/};
// Compiles to:
// @media only screen and (max-width: 48em) {...}
```

##### anti-overlap
Fixes overlaped boundaries of two or more media queries, for example we might have
```css
@media only screen and (max-width: 48em) {/*...*/}
@media only screen and (min-width: 48em) {/*...*/}
```
in this case, when screen size is exactly ```48em's``` or ```768px``` two media queries will be
applied to selector, in order to prevent this override the ```anti-overlap``` value
```scss
$rupture: map-merge($rupture, (
  anti-overlap: true
));
// Resolves to:
@media only screen and (max-width: 48em) {/*...*/}
@media only screen and (min-width: 48.0625em) {/*...*/}
```
By default adjustement happens with ```+1px``` value or ```+0.0625em if``` ```enable-em-breakpoints``` is ```true```, also specific values might be assigned
```scss
anti-overlap: false // default value
anti-overlap: true // enables 1px (or em equivalent) overlap correction globally
anti-overlap: 0px // same as anti-overlap = false
anti-overlap: 1px // same as anti-overlap = true
anti-overlap: -1px // negative offsets decrease the `max-width` arguments
anti-overlap: 0.001em // positive offsets increase the `min-width` arguments
anti-overlap: 1px 0.0625em 0.0625rem // explicit relative values will be used if they are provided instead of calculating them from the font size
```

##### density-queries

Set of vendor prefixes for generating vendor specific density media queries. Valid values are 'webkit', 'moz', 'o', 'ratio', 'dpi', and 'dppx'
Used in ```density()``` and ```retina()``` mixins as well as when ```$density``` or ```$retina``` specified as parameter
```scss
div {
  @include density(2) {/*...*/}
}
// Compiles to
// @media only screen and (min-resolution: 2dppx),
// only screen and (-webkit_min-device-pixel-ratio: 2),
// only screen and (min--moz-device-pixel-ratio: 2),
// only screen and (min-resolution: 192dpi) {
//   div {/*...*/}
// }
```
