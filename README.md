# happy-randomizer
Make your colleagues, friends, and family happy. Tell them how great they are. This is a web application built with a Packer config file.

Check out a [live demo](http://happy.alexandra.space/).

## What's included

This application is a website which randomizes inspirational quotes and GIFs at the click of a button. Included:

+ index.html file
+ JSON file grouping quotes and GIFs
+ CSS stylesheet
+ jQuery for swapping the quotes and GIFs
+ Packer file to build an image on [Triton](https://www.packer.io/docs/builders/triton.html)

## Create a Packer image with Triton

Read my post for Joyent on [creating custom infrastructure images](https://www.joyent.com/blog/create-images-with-packer). The configuration file has already been created, so you can skip ahead to [build the image](https://www.joyent.com/blog/create-images-with-packer#build-the-image).

### Creating image version 1.1.0

There is an alternative JSON file, [thumbsup.json](https://github.com/heyawhite/happy-randomizer/blob/master/resources/thumbsup.json), which uses only thumbs up GIFs. To create another version of a Packer image which uses that JSON file:

1. Edit [main.js](https://github.com/heyawhite/happy-randomizer/blob/master/js/main.js) so that the `url` is equal to `"./resources/thumbsup.json"`.
1. Edit [the Packer configuration](https://github.com/heyawhite/happy-randomizer/blob/master/happy-image.json) to change the `image_version` to 1.1.0.
1. Repeat the instructions for [building an image](https://www.joyent.com/blog/create-images-with-packer#build-the-image)