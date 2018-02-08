# happy-randomizer

Make your colleagues, friends, and family happy. Tell them how great they are. This is a web application built with a Packer config file for [Triton](https://www.packer.io/docs/builders/triton.html).

Check out a [live demo](http://alexandra.space/).

## What's included

This application is a website which randomizes inspirational quotes and GIFs at the click of a button. Included:

+ index.html file
+ JSON file grouping quotes and GIFs
+ CSS stylesheet
+ jQuery for swapping the quotes and GIFs
+ Packer file to build an image on a single Triton data center
+ Packer file to build an image on multiple Triton data centers

## Create a Packer image with Triton

Read my post for Joyent on [creating custom infrastructure images](https://www.joyent.com/blog/create-images-with-packer). The configuration file has already been created, so you can skip ahead to [build the image](https://www.joyent.com/blog/create-images-with-packer#build-the-image).

### Creating image version 1.1.0

There is an alternative JSON file, [thumbsup.json](https://github.com/heyawhite/happy-randomizer/blob/master/resources/thumbsup.json), which uses only thumbs up GIFs. To create another version of a Packer image which uses that JSON file:

1. Edit [main.js](https://github.com/heyawhite/happy-randomizer/blob/master/js/main.js) so that the `url` is equal to `"./resources/thumbsup.json"`.
1. Edit the single data center [Packer configuration file](https://github.com/heyawhite/happy-randomizer/blob/master/happy-image.json) to change the `image_version` to 1.1.0.
   + If deploying to multiple data centers, use that [Packer configuration file](https://github.com/heyawhite/happy-randomizer/blob/master/happy-image-dcs.json) and update the `image_version` in each builder.
1. Repeat the instructions for [building an image](https://www.joyent.com/blog/create-images-with-packer#build-the-image)

### Deploying to multiple data centers

It is possible to deploy your image to multiple data centers at the same time. This requires naming your builder with `"name"` and updating the `"triton_url"`.

In the below example, I'm using a variable for the `us-east-1` and `us-east-2` data centers, both as the name of the builder and as a part of the Triton URL.

```hc1
"variables": {
    "triton_dc_east1": "us-east-1",
    "triton_dc_east2": "us-east-2",
    "triton_account": "{{env `SDC_ACCOUNT`}}",
    "triton_key_id": "{{env `SDC_KEY_ID`}}"
},
"builders": [
    {
      "name": "{{user `triton_dc_east1`}}",
      "type": "triton",
      "triton_url": "https://{{user `triton_dc_east1`}}.api.joyent.com",
      "triton_account": "{{user `triton_account`}}",
      "triton_key_id": "{{user `triton_key_id`}}",
      
      [...]
    },
    {
      "name": "{{user `triton_dc_east2`}}",
      "type": "triton",
      "triton_url": "https://{{user `triton_dc_east2`}}.api.joyent.com",
      "triton_account": "{{user `triton_account`}}",
      "triton_key_id": "{{user `triton_key_id`}}",
      
      "ssh_username": "root",
      
      [...]
    }
],
```

Check out the [Packer file for multiple DCs](https://github.com/heyawhite/happy-randomizer/blob/master/happy-image-dcs.json), which includes all of our available data centers.
