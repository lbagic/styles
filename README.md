Sass utility-oriented framework.

# Installation

- git clone git@github.com:lbzg/styles.git
- point preprocessor to styles/index.scss

Vue example:

```js
// vue.config.js

module.exports = {
  css: {
    loaderOptions: {
      sass: {
        prependData: "@import '@/styles/index.scss';",
      },
    },
  },
}
```

# Usage

Optional configuration:

- `framework/core/colors.scss` - color palette
- `framework/core/breakpoints.scss` - screen breakpoints
- `framework/core/mixin.settings.scss` - output classes vs placeholders & other settings

Usage options:

- use classes directly
- @extend classes
- use `extend mixin`

## Extend mixin

Usage: `@include extend(...params)` where params can be 1) a list, or 2) a map, or 3) a selector and a map.

There are two main benefits of using the extend mixin

- a) programmatic creation of pseudo-classes (ex. @include extend(bg-light--hover))
- b) safe usage and output of class or placeholder prefix (depends on mixin.settings.scss)

Example 1. / params as a list

```scss
.asdf {
  @include extend(bg-light bg-dark--hover);
}
```

Example 2. / params as a map

```scss
$my-map: (
  extend: bg-primary underline,
  card: w-1-3 inline,
  link: (
    base: text-xl color-primary color-grey--visited,
    small: text-s,
  ),
);
.asdf {
  @include extend($my-map);
}
// output: .asdf-card, .asdf-link, .asdf-link-small
// (%asdf-card/item if using placeholders)
```

Reserved keys:

- base - omits key from output class
- extend - all output classes on same level and below extend styles

Example 3. / params as a selector and a list/map

```scss
@include extend(asdf, letter-spacing-1 text-center);
// output: .asdf
// (%asdf if using placeholders)
```

# Classes

## Elements

- background: `bg-{color}`
- border:
  - `border-{color}`
  - `border-{x}` [1, 2] (in px)
- color: `color-{color}`
- cursor: `pointer`, `cursor-default`
- display:
  - `block`, `inline`, `inline-block`, `none`, `initial`, `contents`
  - box-sizing: `box`, `content`
- fill: `fill-{color}`
- fillet (border-radius): `fillet-s`, `fillet`, `fillet-l`, `fillet-xl`, `fillet-rounded`
- flex:
  - type: `flex`, `flex-column`
  - options: `flex-inline`, `flex-wrap`, `flex-grow`
  - justify-content (horizontal): `flex-{x}` [left, right, center, between, around]
  - align-items (vertical): `flex-{x}` [top, bot, mid, stretch]
  - align-content: `flex-content-{x}` [left, right, center, between, around]
  - justify-items: `flex-items-{x}` [left, right, center]
  - item options: `flex-item-{x}`[grow, top, bot, mid, stretch]
- float: `float-{x}` [left, right]
- font: `font-{x}` [sans, serif, mono, pacifico]
- grid:
  - display: `grid`
  - gap: `grid-gap-{x}` [p5, 1, 2, 3] (in rems)
  - columns:
    - fixed: `gtc-{x}` [a, 1, 2, 3, 4, a-1, 1-a] (a as auto)
    - auto fill: `gtc-fill-{x}` [100, 200, 300, 400, 500] (in px / minmax(x, 1fr))
    - auto fit: `gtc-fit-{x}` [100, 200, 300, 400, 500] (in px / minmax(x, 1fr))
  - rows:
    - grid-template-rows: `gtr-{x}` [a, 1, 2, 3, 4]
    - grid-auto-rows: `gtr-auto`, `gtr-auto-max-content`, `gtr-auto-min-content`
- height:
  - `h-{x}` [auto, viewport, 1-1, 1, 2, 5, 10, 50, 100, 150, 200, 250, 300, 400, ..., 1000]
  - min-height: `mh-{x}` ...
- letter-spacing: `letter-spacing-{x}` [0, p5, 1, 2] (in px)
- margin:
  - `m-{x}` [auto, 0, p5, 1, 1p5, 2, 3, 5, 7] (in rems)
  - top: `mt-{x}` ...
  - bottom: `mb-{x}` ...
  - left: `ml-{x}` ...
  - right: `mr-{x}` ...
  - horizontal: `mh-{x}` ...
  - vertical: `mv-{x}` ...
  - **BREAKPOINTS** (ex. s_m-p5, l_m-1)
- opcacity: `opacity-{x}` [0, p25, p5, p75, 1]
- padding:
  - `p-{x}` [auto, 0, p5, 1, 1p5, 2, 3, 5, 7] (in rems)
  - top: `pt-{x}` ...
  - bottom: `pb-{x}` ...
  - left: `pl-{x}` ...
  - right: `pr-{x}` ...
  - horizontal: `ph-{x}` ...
  - vertical: `pv-{x}` ...
- position:
  - `absolute`, `relative`, `fixed`, `static`
  - `top`, `bot`, `left`, `right`, `center`
- box-shadow: `shadow-s`, `shadow`, `shadow-l`, `shadow-xl`, `shadow-inset`, `shadow-none`
- text:
  - style: `bold`, `italic`, `capitalize`, `uppercase`, `lowercase`, `underline`
  - size: `text-xs`, `text-s`, `text`, `text-l`, `text-xl`
  - title-size: `title-xs`, `title-s`, `title`, `title-l`, `title-xl`, `title-xxl`
  - position: `text-{x}` [left, right, center]
  - shadow: `text-shadow`, `text-shadow-dark`
- transition:
  - all: `transition` (ease-in-out, 0.2s)
  - speed: `transition-fast`, `transition-slow`, `transition-xslow`
  - property: `transition-color`, `transition-size`, `transition-position`
- white-space: `white-space-{x}` [normal, nowrap, pre, pre-wrap, pre-line]
- width:
  - `w-{x}` [auto, viewport, 1-1, 1-2, 1-3, 1-4, 1-5, 1-6, 2-3, 2-5, 3-4, 3-5, 4-5, 5-6, 0, 30, 50, 100, 150, 200, 250, 300, 400, ..., 1000] (in px)
  - **BREAKPOINTS** (ex. s_w-1-1, l_w-1-2)
- min-width: `mw-{x}` [1-1, 0, 30, 50, 100, 150, 200, 250, 300, 400, ...., 1000]
- z-index: `z-{x}` [n20, n10, 0, 1, 10, 20, 30, 40, 50, 100, 1000, 9999]

\$colors: `success, warning, danger, primary-darkest, primary-darker, primary-dark, primary, secondary-darkest, secondary-darker, secondary-dark, secondary, tertiary-darkest, tertiary-darker, tertiary-dark, tertiary, grey-darkest, grey-darker, grey-dark, grey, white, light, silver, dark, black, black-t50, transparent, muted`

## Components

- avatars:
  - usage: `avatar-s`, `avatar`, `avatar-l`
- buttons:
  - configure type at `framework/components/buttons.scss` (google, twitter, medium, treehouse)
  - usage:
    - color: `button-{color}` [grey, primary, secondary, tertiary]
    - sizes: `button-s`, `button`, `button-l`, `button-xl`, `button-expand`
- containers:
  - configure max-sizes at `framework/components/container.scss`
  - usage: `container-s`, `container`, `container-l`, `container-xl`, `container-expand`
