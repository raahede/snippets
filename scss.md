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
  @return strip-units($px) / strip-units($base) * 1em;
}
```
