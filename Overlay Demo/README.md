# Home Assistant Image Overlays
This is a working demo with some simple instructions on how to use overlays to light up rooms, or parts of a room.

Tested HA-version found in [.HA_VERSION](./.HA_VERSION)

![demo](./README-images/demo.gif?raw=true)


# Demo in Docker
The [example_docker-compose.yaml](./example_docker-compose.yaml) file can be used to create a container that only expose port 9123 with the web interface.

Copy the config-files in this repo to some path and modify the volume-mapping in the compose-file, renamed to docker-compose.yaml

Then start the container with
```bash
docker-compose up -d
```

Now you should be able to access the demo from http://ip.of.docker.host:9123/


# Sweet Home 3D
You need create your floorplan in [Sweet Home 3D](http://www.sweethome3d.com/), you can probably find tons of guides on how to do that so in this example I will assume that you allready have a nice SH3D-plan to work with.

## Store point of view
To be sure that you create all images from the same 3D viewpoint, store it and name it to something you remember. Then, just before you are about to render an image you can "Go to point of view" and the images will be from the same angle.

![sh3d-store-point](./README-images/sh3d-store-point.png?raw=true)


## Low Lights
The first image you should create is an image with really low lights.
I only use some "Halogen light source" with a power of 5% spread out on the floorplan to give you some lights but not to much. This will be the lights-off image.

## Render Image (Create Photo)
To give you an idea on how the lighting is, start with a medium/high setting for quality and something like 500x500 in size to speed things up.

To give you a dark setting around the floorplan you can adjust the date/time settings.

![sh3d-render-test](./README-images/sh3d-render-test.png?raw=true)


When you have a nice amount of lights it is time to render a high quality image. The size should be a little bit bigger then the screen you are going to use.

This render will take some time to create. Larger image = longer time.

![sh3d-render-max](./README-images/sh3d-render-max.png?raw=true)


Now save the render, this will be your base background image.

![render-lights-off](./README-images/render-lights-off.png?raw=true)

## Render Image
Now, take one light source you want to control (in this case my TV-set) and add lights to it, then render this as a high quality image from the same point of view.

![sh3d-render-tv](./README-images/sh3d-render-tv.png?raw=true)

## Rinse and repeat
Remove the lights from the TV and set lights for some other source or room, then render this... and keep on doing this until you have renders of all objects.

If you have multiple lightsources in one room, render them as separate images.

Yes... this process will take some time.

# Photo Editor
## Mask out
Using a gradient-tool you can mask out the light source for one image.

You can do this with a simple editor like [Paint.net](https://www.getpaint.net/)

![paintnet-gradient](./README-images/paintnet-gradient.png?raw=true)

Starting with an image from SH3D with the whole floorplan where you have one (or more) objects with light.

![render-tv](./README-images/render-tv.png?raw=true)

Masking out the light source with a gradient to transparent will give you a nice overlay that lights up the area.
Keep the image size the same and just delete (transparent) the background and unnecessary parts.

![render-tv-masked](./README-images/render-tv-masked.png?raw=true)

Work with all images and mask out the different sources, in the kitchen you can clearly see a combination of hard and gradient masks. Hard where there are walls and gradient to control the light.

![render-kitchen-masked](./README-images/render-kitchen-masked.png?raw=true)

## Tip
When rendering images, if you have multiple lightsources that doesn't interfere with each other you can render them all in one go.

In this example I render a few rooms that have closed doors between them, the amount of time that SH3D will take to render this is the same as it would for an image with lights on in a single room.

![render-multiple](./README-images/render-multiple.png?raw=true)


# Home Assistant

### Placement

In the file [ui-demo.yaml](./ui-demo.yaml) you will find some anchors, first of is ***backgroundImagesPlacement*** that controls the placement of the background image and all overlays.

It will take some time to get this right...

When creating you own, start with left & top at 50% and the size at 100%

To get the whole images I actually use left 45%, top 36% and a size of 118%

```yaml
anchors:
  <<: &backgroundImagesPlacement
    left: 30%
    top: 16%
    width: 150%
```
### Overlays

A bit down is the start of all overlays and icons, here is an example of the overlay for the library.

This is a conditional image that will show if the ***input_boolean.overlay_library*** has the state ***on***, in a real envionment you probably will change that to something like ***light.library***

```yaml
- type: conditional
    conditions:
    - entity: input_boolean.overlay_library
        state: 'on'
    elements:
    - image: /local/overlay-library.png
    <<: *defaultOverlay
```

### Icons

And then you have a button for the "light", in the example this is ***input_boolean.overlay_library***

```yaml
- type: image
    entity: input_boolean.overlay_library
    <<: *defaultLightButton
    style:
    <<: *iconsCommonStyle
    left: 24%
    top: 32%
```

Most of the settings are controlled via some yaml-anchors at the top

```yaml
- type: image
  entity: input_boolean.overlay_library
  <<: *defaultLightButton
  style:
    <<: *iconsCommonStyle
    left: 24%
    top: 32%
```

If you want to do this without the anchors it would look something like this

```yaml
- type: image
  entity: input_boolean.overlay_library
  tap_action:
    action: toggle
  hold_action:
    action: none
  state_image:
    "on": /local/icon-light-on.png
    "off": /local/icon-light-off.png
  state_filter:
    "on": opacity(1)
    "off": opacity(0.7)
  style:
    max-width: 5.5%
    transform: 'scale(0.75,0.75)'
    z-index: 10
    left: 24%
    top: 32%
```
