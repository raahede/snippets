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
