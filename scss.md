SCSS
====

### Clearfix

``` SCSS
%clearfix {
  // For modern browsers
  // 1. The space content is one way to avoid an Opera bug when the
  //    contenteditable attribute is included anywhere else in the document.
  //    Otherwise it causes space to appear at the top and bottom of elements
  //    that are clearfixed.
  // 2. The use of 'table' rather than 'block' is only necessary if using
  //    ':before' to contain the top-margins of child elements.
  &:before,
  &:after {
    content: " ";
    display: table;
  }

  &:after {
    clear: both;
  }

  // For IE 6/7 only
  *zoom: 1;
}
```

### Clean list

``` SCSS
%list-clean {
  list-style: none;
  margin-bottom: 0;
  margin-top: 0;
  padding-left: 0;
}
```

### CSS character bullets

``` SCSS
%bullet-list {
  @extend %list-clean;
  li {
    &:before {
      content:"\2022";
      display: inline-block;
      margin: 0 0.5em 0 0.2em;
    }
    @media (min-width: 481px) {
      display: inline-block;
      &:first-child:before {
        display: none;
      }
    }
  }
}
```

### Font icons

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
``` SCSS
// Icon map
// Name         Icon    Size
$icons : (
  arrow-right : "\e600" 18px,
  close       : "\e601" 18px,
  comment     : "\e602" 18px,
  contact     : "\e603" 18px,
  error       : "\e604" 18px,
  home        : "\e605" 18px,
  open        : "\e606" 18px,
  pdf         : "\e607" 18px,
  pin         : "\e608" 18px,
  tick        : "\e609" 18px,
  user        : "\e60a" 18px,
  watch       : "\e60b" 18px
);
```
``` SCSS
// Icon styles
%icon {
  font-family: "icons";
  font-style: normal;
  font-variant: normal;
  font-weight: normal;
  speak: none;
  text-transform: none;

  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
}

@mixin font-icon( $icon-letter ) {
  @extend %icon;
  content: "#{ $icon-letter }";
}

// Creating icons from map
@each $name, $params in $icons {
  $letter : nth( $params, 1 );
  $size   : nth( $params, 2 );

  // Creating placeholder selectors for pseudo elements to extend
  // .my-class { @extend %icon--close }
  %icon--#{ $name } {
    @include font-icon( $letter );
    @include font-size( $size );
  }

  // Creating classes for use in the DOM
  // <p class="icon--close">Close</p>
  .icon--#{ $name }:before {
    @extend %icon--#{ $name };
  }
}

// Icons for use with data-attribute
// <p class="icon" data-icon="\e601">Close</p>
.icon {
  &:before {
    @extend %icon;
    content: attr(data-icon);
  }
}
```
``` HTML
<!-- Add icons in your markup -->
<p class="icon" data-icon="\e600">Arrow right</p>
<p class="icon--arrow-right">Arrow right</p>
<p class="icon" data-icon="\e601">Close</p>
<p class="icon--close">Close</p>
```

