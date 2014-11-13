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

### Alternative text on media query

```HTML
<a href="/" class="button" title="Text on mobile">Text on desktop</a>
```

```SCSS
.button {
  display: inline-block;
  float: left;
  @media (max-width: 1079px) {
    text-indent: -999px;
    overflow: hidden;
    &:before {
      content: attr(title);
      display: inline;
      text-indent: 0;
      float: inherit;
    }
  }
}

// Alternative: not dependent on floating the button
.button {
  display: inline-block;
  @media (max-width: 1079px) {
    font-size:0;
    overflow: hidden;
    &:before {
      font-size: 16px;
      content: attr(title);
      display: inline;
      float: inherit;
    }
  }
}
```

### Center vertical

``` SCSS
// Container
%center-vertical-container {
  @media (min-width: 481px) {
    display: table;
    width: 100%;
  }
}

// Center text vertical
%center-vertical {
  @media (min-width: 481px) {
    display: table-cell;
    vertical-align: middle;
  }
}

// Center text vertical - right
%center-vertical--right {
  @extend %center-vertical;
  @media (min-width: 481px) {
    text-align: right;
  }
}

// Show first on mobile
%center-vertical--top {
  @extend %center-vertical;
  display: table-header-group;
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

### Navigation bullets

``` SCSS
$bullet-size: 12px !default;
$bullet-background: #fff !default;
$bullet-background-selected: $bullet-background !default;
$bullet-color: #cdcdcd !default;
$bullet-color-selected: #000 !default;

%bullet {
  background-color: $bullet-background;
  border: ceil($bullet-size / 6) solid $bullet-color;
  cursor: pointer;
  display: inline-block;
  height: $bullet-size;
  overflow: hidden;
  position: relative;
  text-indent: -100%;
  width: $bullet-size;
  @include border-radius(100%);

  &:after {
    background-color: $bullet-color-selected;
    content: "";
    display: none;
    position: absolute;
      bottom: ceil($bullet-size / 4);
      left: ceil($bullet-size / 4);
      right: ceil($bullet-size / 4);
      top: ceil($bullet-size / 4);
    @include border-radius(100%);
  }
}

%bullet--selected {
  background-color: $bullet-background-selected;
  border-color: $bullet-color-selected;

  &:after { display: block; }
}
```

### Round down helper

``` SCSS
// Round down numbers with or without units
@function get-floor( $number ) {
  @if( unitless( $number ) ) {
    @return floor( $number );
  } @else {
    $unit: $number * 0 + 1;
    $unitless: $number / ($number * 0 + 1);
    @return floor( $unitless ) * $unit;
  }
}
```
### Strip units helper

``` SCSS
@function strip-units($number) {
  @return $number / ($number * 0 + 1);
}
```
### Pixels to EMs helper

``` SCSS
@function px-to-em( $px, $base: 16px ) {
  @if type-of( $px ) == "list" {
    $output: ();
    @each $value in $px {
      $output: append($output, px-to-em( $value, $base ));
    }
    @return $output;
  } @else {
    @return strip-units($px) / strip-units($base) * 1em;
  }
}
```
