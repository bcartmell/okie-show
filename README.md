# Okie-Show

A slideshow Plugin.  Contructor OkieShow(options) accepts an options object as 
an argument.  It requires okie-helpers and okie-pub.  It can optionally
be used with okie-modal to load the slideshow in a modal.

Okie-show can link thumbnails on the page to the fullsize version that will
be displayed in the slideshow.  To use this functionality, add the boolean
attribute data-thumb to the thumbnail image and add the url to the fullsize
image to the data-fullsize attribute.

## options

animationType - defaults to 'fade', currently fade is the only option

spinnerClass - className to add for the loading icon defaults to 'fa fa-spinner' (font-awesome spinner icon)

## Example usage

This example uses the className 'image-reel-window' as the container for image
thumbnails.  Multiple '.image-reel-window' containers can exist on a single page.

      var reel = [selector for thumbnail container]

      // create on object to keep track of the container element and each image.
      var reelObj =  {
        element: reel,
        images: [].filter.call(reel.childNodes, function(node) {
          return node instanceof Element;
        })
      };

      // use okiePub to make our new object a publisher
      okiePub.makePublisher(reelObj);

      // on click event fire an 'imgClick' event via okiePub functionality
      // the event will include the index of the of thumbnail that was clicked.
      reel.addEventListener('click', function(event) {
        reelObj.fire('imgClick', [].indexOf.call(reelObj.images, event.target));
      });

      // create the okieShow object and save it in our tracking object
      reelObj.targetShow = new OkieShow();

      // Clone the thumbnail into a new slide on the OkieShow instance.
      reelObj.images.forEach(function(image) {
        var slideContent = image.cloneNode(false);
        reelObj.targetShow.addSlide(slideContent);
      });

      // the slideshow can be loaded in a modal
      reelObj.modal = okieModal.newModal({
        content: reelObj.targetShow
      });

      // add subscriptions to to the 'imgClick' event
      // which is fired when a thumbnail is clicked.  
      // one subcription to navigate the slideshow to the correct image.
      // and onother to open the modal.
      reelObj.on('imgClick', reelObj.targetShow.toSlide, reelObj.targetShow);
      reelObj.on('imgClick', reelObj.modal.show, reelObj.modal);
