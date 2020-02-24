Sass utility-oriented framework with helper functions for writing more concise and organized code. Uses placeholder selectors to limit css footprint to only used classes.

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
`framewrok/generator/...` - generator & extender mixins & settings  
`framewrok/elements/...` - base elements  
`framewrok/components/...` - components built by extending base elements  
`components/...` - place for webpage specific components

## Info

Framework **elements are built with `generator mixins`** that generate css placeholders (or classes; configurable in _generator.settings.scss_).

Framework (and user) **components are created with `extender mixins`**.

**Use extender mixins (`extendMap` & `extendList`) for creating custom webpage component styles!**

### Generator mixins

`generate`(selector, property, map, options?)  
`generateProps`(selector, map, options?)  
`generateSelectors`(selectorMap, options?)

- _generate and generateProps support nested keys_
- _'base' key is reserved / attaches directly to selector_
- _available options: **hover active disabled focus breakpoints**_

```scss
// generate map example (key -> value)
// framework/elements/fillet.scss

$fillet-map: (
  small: 0.125rem,
  base: 0.25rem,
  large: (
    base: 0.5rem,
    2: 1rem,
    3: 2rem,
  ),
  full: 9999px,
);

@include generate(fillet, border-radius, $fillet-map);

// output
%fillet-small {
  border-radius: 0.125rem;
}
%fillet {
  border-radius: 0.25rem;
}
%fillet-large {
  border-radius: 0.5rem;
}
%fillet-large-2 {
  border-radius: 1rem;
}
%fillet-large-3 {
  border-radius: 2rem;
}
%fillet-full {
  border-radius: 9999px;
}


// example with options
@include generate(fillet, border-radius, $fillet-map, hover breakpoints);

// hover output
%fillet-small--hover:hover {
  border-radius: 0.125rem;
} ...
// breakpoints output
@screen (min-width: $breakpoint-small) {
  %s_fillet-small {
    border-radius: 0.125rem;
  }
} ...
```

```scss
// generateProp map example (css-property -> key -> value)
// framework/elements/display.scss

$display-prop-map: (
  display: (
    block: block,
    inline-block: inline-block,
    none: none,
    initial: initial,
  ),
  box-sizing: (
    box: border-box,
  ),
  ...
);

@include generateProps(display, $display-prop-map);
...
```

```scss
// generateSelectors map example (selector -> css-property -> value)
// framework/elements/text.scss

$text-selector-map: (
  bold: (
    font-weight: bold,
  ),
  italic: (
    font-style: italic,
  ),
  capitalize: (
    text-transform: capitalize,
  ),
  uppercase: (
    text-transform: uppercase,
  ),
  ...
);

@include generateSelectors($text-selector-map);
...
```

### Extender mixins

`extend`(selector, extensions, baseExtensions?) - _best used for framework components_  
`extendMap`(map, baseExtensions?)  
`extendList`(list)

- _extendMap & extendList need to be wrapped in selector when called_
- _support nested keys_
- _'base' key is reserved / attaches directly to selector_
- _'extend' key is reserved / extends class/placeholder to all other keys_

```scss
// extend mixin
$button-colors: (
  extend: transition-quick transition-bg fillet-small shadow-medium,
  blue: bg-blue bg-blue-500--hover bg-blue-600--active color-white,
  red: bg-red bg-red-600--hover bg-red-700--active color-white,
);

@include extend(button, $button-colors);

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
// extendMap mixin
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
// extendList mixin
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
