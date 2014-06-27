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
@mixin font-icon( $icon-letter ) {
  @extend %icon;
  content: "#{ $icon-letter }";
}

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

%icon {
  font-family: 'icons';
  font-style: normal;
  font-variant: normal;
  font-weight: normal;
  line-height: 1;
  position: relative;
    top: 1px;
  speak: none;
  text-transform: none;

  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;

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

