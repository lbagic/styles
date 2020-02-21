Sass utility-oriented framework with helper functions for writing more concise and organized code.

## Installation

Clone (into your assets folder) & point your sass loaders to index.scss.  
Make sure to install [sass-loader](https://github.com/webpack-contrib/sass-loader) and [node-sass](https://github.com/sass/node-sass) or similar (ex. [dart-sass](https://github.com/sass/dart-sass)).

```sh
git clone https://github.com/lbzg/styles.git
```

Example setup with Vue.js via `vue.config.js`:

```sh
module.exports = {
  css: {
    loaderOptions: {
      sass: {
        prependData: "@import '@/assets/styles';",
      },
    },
  },
}
```

## Content

`index.scss` - entry point  
`base.scss` - base webpage styles  
`framewrok/variables/...` - colors and screen breakpoints  
`framewrok/generator/...` - core functionality for generating and extending styles  
`framewrok/generator/generator.config.scss` - configure output to placeholders (%) or classes (.)  
`framewrok/...` - base style elements  
`components/...` - components built by extending base elements

### Info

Core mixins **generate, generateProps, generateSelectors & extend** take selector as a parameter; selector type is then contained and controlled through `generator.config.scss`. _Theses mixins are best used within framework and component elements._

Other two mixins **extendList** & **extendMap** need a to be wrapped around selector when invoked. _These mixins are best used to create custom one-off styles._

### **Core functions / generators**

_Used within framework to generate utility classes._

`generate`(selector, property, map, options?)  
`generateProps`(selector, map, options?)  
`generateSelectors`(selectorMap, options?)

Options: **hover active disabled focus breakpoints**  
Special map keys: **base** (applied to base selector)

Example:

```scss
// MIXIN: generate
$text-map-title: (
  small: 1.5rem,
  base: 2rem,
);

@include generate(title, font-size, $text-map-title);

// output
%title {
  // base key attaches directly to selector
  font-size: 2rem;
}
%title-small {
  font-size: 1.5rem;
}
```

```scss
// MIXIN: generateProps
$text-prop-map: (
  font-size: (
    base: 1rem,
    large: 1.25rem,
  ),
  text-align: (
    right: right,
    center: center,
  ),
);

@include generateProps(text, $text-prop-map);

// output
%text {
  font-size: 1rem;
}
%text-large {
  font-size: 1.25rem;
}
%text-right {
  text-align: right;
}
%text-center {
  text-align: center;
}
```

```scss
// MIXIN: generateSelectors
$text-selector-map: (
  bold: (
    font-weight: bold,
  ),
  uppercase: (
    text-transform: uppercase,
  ),
);

@include generateSelectors($text-selector-map);

// output
%bold {
  font-weight: bold;
}
%uppercase {
  text-transform: uppercase;
}
```

```scss
// multiple OPTIONS example:
@include generate(title, font-size, $text-map-title, hover breakpoints);

// hover/active/disabled/focus example output
%title-small--hover:hover {
  font-size: 1.5rem;
}

// breakpoint s (small) example output
@media (min-width: $breakpoint-small) {
  %s_title-small {
    font-size: 1.5rem;
  }
}
```

### **Core functions / extenders**

_Used to build custom components and style classes._

`extend`(selector, extensions, baseExtensions?)  
`extendMap`(map, baseExtensions?)  
`extendList`(list)

Special map keys: **base** (applied to base selector), **extend** (apples to all other keys)

```scss
// MIXIN: extend
// best used for components, wraps
$button-colors: (
  extend: transition-quick transition-bg fillet-small shadow-medium,
  blue: bg-blue bg-blue-500--hover bg-blue-600--active color-white,
  red: bg-red bg-red-600--hover bg-red-700--active color-white,
);

$button-sizes: (
  extend: text uppercase letter-spacing-1,
  auto: ph-2 pv-1 text-medium,
  tiny: w-100 pv-1 text-medium,
  small: w-150 pv-2,
  medium: w-250 pv-2p5,
  large: w-350 pv-3,
);

@include extend(button, $button-colors);
@include extend(button, $button-sizes);

// outputs
%button-blue {...}
%button-red {...}
%button-auto {...}
%button-tiny {...}
%button-small {...}
%button-medium {...}
%button-large {...}
```

```scss
// MIXIN: extendMap
$nav: (
  base: text-large bg-black-transparent-50 pv-0p5 ph-2 l_ph-4,
  wrap: flex flex-mid,
  logo: mr-auto title font-pacifico,
  item: ml-2 font-pacifico,
  link: decoration-none decoration-none--hover,
  button: bg-orange-300 bg-orange--hover bg-orange-500--active ph-0p75
    pv-0p25 transition transition-bg,
);

.nav {
  @include extendMap($nav);
}

// output
.nav {...}
.nav-wrap {...}
.nav-logo {...}
.nav-item {...}
.nav-link {...}
.nav-button {...}
```

```scss
// MIXIN: extendList
.element {
  @include extendList(bg-white color-dark w-1-1);
}

// output
.element {
  background: #fff;
  color: #212121;
  width: 100%;
}
```

## Framework elements

todo...

## Configuration

`base.scss` - basic webpage styles  
`framework/variables/colors.scss` - colors (comment out unneded to reduce compile time)  
`framework/variables/breakpoints.scss` - screen breakpoints  
`framework/...elements.scss` - config styles and options on each element (like breakpoints/hover/active/disabled/focus...)  
`framework/generator/generator.settings.scss` - generator options for selector type and delimiters for breakpoints, states etc...
