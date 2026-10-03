# SpaceSnap AI: What's Happening in This Space Image?

A browser tool that analyses a NASA or Earth-observation image, finds the main visible features, highlights them on the image, and explains the scene in simple language.

**Live demo:** https://archit-ravikumar.github.io/SpaceSnap-AI/

## Pipeline

Image -> Feature detection -> Explanation

1. **Image:** pick a sample image or upload your own.
2. **Feature detection:** features are shown as a colour overlay with labels and percentages. Each feature can be switched on or off, and the overlay strength can be adjusted.
3. **Explanation:** a short plain-language description of the scene, plus a one-line meaning for each detected feature.

## Features it can detect

Clouds / ice, ocean / water, forest / vegetation, desert / dry land, land / terrain, gray rocky surface, fire / hot spots, city lights, and dark space / shadow. At least 3 are found in a typical Earth image.

## Approach

SpaceSnap AI runs entirely in the browser. A loaded image is shrunk to about 240 pixels, and k-means clustering, an unsupervised machine-learning method, groups its pixels into six colour clusters. Each cluster's average colour is converted to hue, saturation and brightness and matched against simple rules to name it: bright, low-saturation clusters become clouds or ice, dark blue becomes ocean, green becomes vegetation, tan becomes desert, and so on. A smoothing filter removes speckle, then each feature is highlighted as a colour overlay with a label showing how much of the image it covers. Finally, a template writes a plain-language explanation from the detected features and their proportions. It is a lightweight colour-based classifier, so it can confuse clouds with snow or ice.

## Example output

For an Earth image with a storm over the ocean:

> Detected: Ocean / water, Clouds / ice, Forest / vegetation.
> This image is mostly ocean or water (50%), with clouds (or ice) (28.1%) and forest or vegetation (21.9%). Clouds are scattered over the water.

## Sample images and source

Sample images are in the `samples/` folder and come from NASA (public domain):

| File | Image | Source |
|---|---|---|
| `samples/blue-marble.jpg` | Earth, Blue Marble | NASA Visible Earth (visibleearth.nasa.gov) |
| `samples/hurricane.jpg` | Hurricane / storm over ocean | NASA Earth Observatory (earthobservatory.nasa.gov) |
| `samples/forest.jpg` | Forest and river | NASA Earth Observatory |
| `samples/ice.jpg` | Polar ice | NASA Visible Earth |

Credit: NASA.

## Limitations

This is a colour-based estimate, not a trained deep-learning model. It can mix up clouds with snow or ice, and it cannot find objects such as craters by shape. A future version could replace the colour rules with a trained segmentation model.
