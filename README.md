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
  base-font-size: 16px,
  anti-overlap: false,
  density-queries: 'dppx' 'webkit' 'moz' 'dpi',
  retina-density: 1.5,
  use-device-width: false,
  rasterise-media-queries: false,
);
```

In order to override any value use ```$rupture: map-merge($rupture, (key: value, ..., keyN: valueN)```

```scss
// e.g.
$rupture: map-merge($rupture, (
  mobile-cutoff: 767px,
  desktop-cutoff: 1365px
));
```

##### mobile-cutoff
Upper bound where `mobile()` mixin has an effect and lower bound of the `tablet()` mixin.

##### desktop-cutoff
Lower bound where `desktop()` mixin has an effect and upper bound of the `tablet()` mixins.

##### hd-cutoff
Lower bound where `hd()` mixin starts to have an affect

<!-- ##### scale
A list of values that you can reference by index in most of the mixins listed below. This works exactly like [breakpoint-slicer](https://github.com/lolmaus/breakpoint-slicer). Default looks like this:
```
scale: 0 400px 600px 800px 1050px 1800px
```
##### scale-names
A list of strings you can reference that correspond to their index location in `scale`. This works exactly like [breakpoint-slicer](https://github.com/lolmaus/breakpoint-slicer#calling-slices-by-names-rather-than-numbers)

```
scale:                      0         400px       600px       800px       1050px     1800px
//                     └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘ └────┬────
// Slice numbers:           1           2           3           4           5           6
scale-names:               'xs'        's'         'm'         'l'         'xl'        'hd'
```

##### enable-em-breakpoints
Enables Rupture's [PX to EM unit conversion](#px-to-em-unit-conversion) feature. If set to true, pixel breakpoint values will be automatically converted into em values.
 -->
