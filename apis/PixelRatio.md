PixelRatio class gives access to the device pixel density.

There are a few use cases for using PixelRatio:

## Displaying a line that's as thin as the device permits 

A width of 1 is actually pretty thick on an iPhone 4+, we can do one that's thinner using a width of `1 / PixelRatio.get()`. It's a technique that works on all the devices independent of their pixel density.

```javascript
style={{ borderWidth: 1 / PixelRatio.get() }}
```

## Fetching a correctly sized image 

You should get a higher resolution image if you are on a high pixel density device. A good rule of thumb is to multiply the size of the image you display by the pixel ratio.

```javascript
var image = getImage({
  width: 200 * PixelRatio.get(),
  height: 100 * PixelRatio.get()
});
<Image source={image} style={{width: 200, height: 100}} />
```

## Methods 

static **get**() 

Returns the device pixel density. Some examples:

* PixelRatio.get() === 2
    - iPhone 4, 4S
    - iPhone 5, 5c, 5s
    - iPhone 6
* PixelRatio.get() === 3
    - iPhone 6 plus

## Description 

# Pixel Grid Snapping 

In iOS, you can specify positions and dimensions for elements with arbitrary precision, for example 29.674825. But, ultimately the physical display only have a fixed number of pixels, for example 640×960 for iphone 4 or 750×1334 for iphone 6. iOS tries to be as faithful as possible to the user value by spreading one original pixel into multiple ones to trick the eye. The downside of this technique is that it makes the resulting element look blurry.

In practice, we found out that developers do not want this feature and they have to work around it by doing manual rounding in order to avoid having blurry elements. In React Native, we are rounding all the pixels automatically.

We have to be careful when to do this rounding. You never want to work with rounded and unrounded values at the same time as you're going to accumulate rounding errors. Having even one rounding error is deadly because a one pixel border may vanish or be twice as big.

In React Native, everything in JS and within the layout engine work with arbitrary precision numbers. It's only when we set the position and dimensions of the native element on the main thread that we round. Also, rounding is done relative to the root rather than the parent, again to avoid accumulating rounding errors.