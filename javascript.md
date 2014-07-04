JavaScript
==========

## Scroll anchor links
``` js
$('.js-anchor-link').on( 'click', function( event ) {
  var targetId = $(this).attr('href'),
      $target = $( targetId );
  // Animate scroll to target if it exists
  if( $target.length !== 0 ) {
    event.preventDefault();
    $('html, body').animate(
      { scrollTop: $target.offset().top },
      500
    );
  }
});
```

## Test for CSS3 Transition support
``` js
function supportTransition() {
  var _body = document.body || document.documentElement,
      _bstyle = _body.style,
      _transition = [ _bstyle.transition, _bstyle.WebkitTransition, _bstyle.MozTransition, _bstyle.MsTransition, _bstyle.OTransition ];
  for (var i = 0; i < _transition.length; i++) {
    if( typeof( _transition[i] ) !== 'undefined' ) {
      return true;
    }
  };
  return false;
}
```
