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
$icons : (
  arrow-right : "\e600",
  close       : "\e601",
  comment     : "\e602",
  contact     : "\e603",
  error       : "\e604",
  home        : "\e605",
  open        : "\e606",
  pdf         : "\e607",
  pin         : "\e608",
  tick        : "\e609",
  user        : "\e60a"
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

.icon {
  &:before {
    @extend %icon;
    content: attr(data-icon);
  }
  @each $name, $letter in $icons {
    &--#{ $name } {
      &:before {
        @include font-icon( $letter );
      }
    }
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

