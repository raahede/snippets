# Font icons

**Include font **

``` SCSS
// Include font (using Compass font-url helper)
@font-face {
    font-family: "icons";
    src: font-url("icons.eot");
    src: font-url("icons.eot?#iefix") format("embedded-opentype"),
         font-url("icons.woff") format("woff"),
         font-url("icons.ttf") format("truetype"),
         font-url("icons.svg#icons") format("svg");
}
```
**Settings**
``` SCSS
// Icon font family
$icon-font: "icons" !default;

// --------
// Icon map
// Properties:
// name     character       font size    color
$icons : (
  plus  : ( char : "\e603", size : 14px, color : #008000 ),
  minus : ( char : "\e605", size : 14px, color : #F00 )
) !default;

```
**Default use**
``` SCSS
// Extending icon selector
.button { @extend .icon--button; }

// Extending placeholder selector (must be applied to pseudo element)
.button {
  &:before { @extend %icon--button; }
}

// Using mixin (must be applied to pseudo element)
// Extends a placeholder selector %icon--button
.button {
  &:before { @include use-icon( button ); }
}

// Icon only (text is hidden)
.arrow {
  @extend %icon-only;
  @extend .icon--arrow;
}
```
**Use inside media query**
``` SCSS
// Forcing style output rather than extending a placeholder selector
.button:before {
  @include susy-breakpoint( $bp-l-xl ) {
    @include use-icon( button, true );
  }
}
```
**Using default output**
``` HTML
<!-- Add icons in your markup -->
<p class="icon" data-icon="\e600">Arrow right</p>
<p class="icon--arrow-right">Arrow right</p>
<p class="icon" data-icon="\e601">Close</p>
<p class="icon--close">Close</p>
```

## Source

``` SCSS
// ---------------------------------------------------------------------------
// Defaults
// These variables should be overridden in your settings partial

// Icon font family
$icon-font: "icons" !default;

// --------
// Icon map
// Properties:
// name     character       font size    color
$icons : (
  plus  : ( char : "\e603", size : 14px, color : #008000 ),
  minus : ( char : "\e605", size : 14px, color : #F00 )
) !default;

// ---------------------------------------------------------------------------
// Default usage

/*

// Extending icon selector
.button { @extend .icon--button; }

// Extending placeholder selector (must be applied to pseudo element)
.button {
  &:before { @extend %icon--button; }
}

// Using mixin (must be applied to pseudo element)
// Extends a placeholder selector %icon--button
.button {
  &:before { @include use-icon( button ); }
}

// Icon only (text is hidden)
.arrow {
  @extend %icon-only;
  @extend .icon--arrow;
}

*/

// ---------------------------------------------------------------------------
// Use inside media query

/*

// Forcing style output rather than extending a placeholder selector
.button:before {
  @include susy-breakpoint( $bp-l-xl ) {
    @include use-icon( button, true );
  }
}

*/

// ---------------------------------------------------------------------------
// Using icons in BEM syntax

/*
In most cases it's not necessary to use all available icons in a given syntax.
This way, we can hand pick the icons we want to use:

SCSS
====

.footer {
  &__icon {
    @each $name in facebook, instagram {
      &--#{ $name } {
        @extend .icon--#{ $name };
      }
    }
  }
}

Output
======

.footer__icon--facebook { ... }
.footer__icon--instagram { ... }

*/


// ---------------------------------------------------------------------------
// Icon helpers

/**
 * Returns an icon map base on its name
 * If a property is provided, the value of that property is returned
 * @type  {function}
 * @param {String}  $name       [required] icon name
 * @param {String}  $property   [optional] property name
 */
@function get-icon( $name, $property: false ) {
  @if not $property {
    @return map-get( $icons, $name );
  } @else {
    @return map-get( map-get( $icons, $name ), $property );
  }
}

/**
 * Set icon
 * Can be used without $icons map
 * Must be applied to a pseudo element (:before, :after)
 * @type  {mixin}
 * @param {String}  $icon-letter  [required] hexadecimal letter for the icon
 * @param {Bool}    $force        [optional] if set to true, the icon styles
 *                                           are rendered inline rather than
 *                                           through @extend (for embedding
 *                                           in media queries)
 */
@mixin set-icon( $icon-letter, $force: false ) {
  content: "#{ $icon-letter }";
  @if $force {
    @include icon-base;
  } @else {
    @extend %icon;
  }
}

/**
 * Use icon
 * Requires $icons map to be defined
 * Must be applied to a pseudo element (:before, :after)
 * @type  {mixin}
 * @param {String}  $icon-name    [required] name of icon to use
 * @param {Bool}    $force        [optional] if set to true, the icon styles
 *                                           are rendered inline rather than
 *                                           through @extend (for embedding
 *                                           in media queries)
 */
@mixin use-icon( $icon-name, $force: false ) {
  @if $force {
    @include set-icon( get-icon( $icon-name, char ), $force );
  } @else {
    @extend %icon--#{ $name };
  }
}

// ---------------------------------------------------------------------------
// Icon base styles

@mixin icon-base {
  font: {
    family: $icon-font;
    style: normal;
    variant: normal;
    weight: normal;
  }
  height: 1em;
  line-height: 1em;
  speak: none;
  text-indent: 0; // avoiding offset in conjuction with %icon-only
  text-transform: none;

  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
}

// ---------------------------------------------------------------------------
// Icon placeholder styles

%icon {
  @include icon-base;
}

%icon-only {
  text-indent: -999px;
  overflow: hidden;
  display: inline-block;
  // Fixing view if element is floated
  &:before,
  &:after {
    float: inherit;
  }
}

// ---------------------------------------------------------------------------
// Creating icon placeholders/classes

// Creating icons from map
@each $name, $settings in $icons {
  $letter : map-get( $settings, char );
  $size   : map-get( $settings, size );
  $color  : map-get( $settings, color );

  // Creating placeholder selectors for pseudo elements to extend
  // .my-class:before { @extend %icon--close }
  %icon--#{ $name } {
    @if $color { color: $color; }
    @include set-icon( $letter );
    @include font-size( $size );
  }

  // Creating classes for use in the DOM
  // <p class="icon--close">Close</p>
  .icon--#{ $name }:before { @extend %icon--#{ $name }; }
}

// Icons for use with data-attribute
// <p class="icon" data-icon="\e601">Close</p>
.icon {
  &:before {
    @extend %icon;
    content: attr(data-icon);
  }
}

// ---------------------------------------------------------------------------
// Text styles to use with icons

%link-icon { text-decoration: none; }

%text-icon {
  position: relative;
  @include rem( margin-right, 6px );
}

// <p class="text-icon--clock">Fra kl. 13:00</p>
// <a href="" class="link-icon--pin"><span class="link-icon__text">Show on map</span></a>
@each $name, $settings in $icons {
  $size : map-get( $settings, size );
  .text-icon--#{ $name } {
    &:before {
      @extend %text-icon;
      @extend %icon--#{ $name };
      bottom: -(get-floor( $size / 5 ));
    }
  }
  .link-icon--#{ $name } {
    @extend %link-icon;
    @extend .text-icon--#{ $name };
  }
}

// Create a placeholder selector "%link" for link styles
.link-icon__text { @extend %link !optional; }

```
