+++
title = "Read EXIF data with Ubuntu"
date = "2019-07-10"
layout = ["article"]
author = "soeren"
tags = ["fotographie", "ubuntu", "open-source"]
+++

Today only very briefly, because I just searched for it, [found it](https://mikebeach.org/2011/12/17/how-to-retrieve-exif-data-from-the-command-line-in-ubuntu-linux/) and probably will forget it tomorrow: With the help of `imagemagick` and [Ubuntu](/tags/ubuntu/) you can read the exif-data of your [photos](/tags/fotographie).

### `imagemagick` installieren

```
batman@helium:~$ sudo apt-get install imagemagick
```

### Use `identify` to read the meta data

```
batman@helium:~$ identify -verbose link/to/image.jpg

Image: image.jpg
  Format: JPEG (Joint Photographic Experts Group JFIF format)
  Mime type: image/jpeg
  Class: DirectClass
  Geometry: 458x458+0+0
  Resolution: 72x72
  Print size: 6.36111x6.36111
  Units: PixelsPerInch
  Colorspace: sRGB
  Type: TrueColor
  Base type: Undefined
  Endianess: Undefined
  Depth: 8-bit
  Channel depth:
    red: 8-bit
    green: 8-bit
    blue: 8-bit
  ...
```

### Restrict output to the exif information

If you are only interested in the exif information, you can limit the output using `grep`. 

```
batman@helium:~$ identify -verbose link/to/image.jpg | grep "exif:"

    exif:ApertureValue: 177537/78038
    exif:BrightnessValue: 128447/18170
    exif:ColorSpace: 1
    exif:ComponentsConfiguration: 1, 2, 3, 0
    exif:ExifImageLength: 458
    exif:ExifImageWidth: 458
    exif:ExifOffset: 170
    exif:ExifVersion: 48, 50, 50, 49
    exif:ExposureBiasValue: 0/1
    exif:ExposureMode: 0
    ...
```

### Extract geo coordinates

And if, for example, you are only interested in the geo-coordinates in the photo, you can of course also search for them. 

```
batman@helium:~$ identify -verbose link/to/image.jpg | grep "exif:GPS"

    exif:GPSAltitude: 125791/8327
    exif:GPSAltitudeRef: 0
    exif:GPSDateStamp: 2018:10:26
    exif:GPSDestBearing: 268263/3437
    exif:GPSDestBearingRef: T
    exif:GPSImgDirection: 611324/2369
    exif:GPSImgDirectionRef: T
    exif:GPSInfo: 1670
    exif:GPSLatitude: 35/1, 43/1, 5997/100
    exif:GPSLatitudeRef: N
    exif:GPSLongitude: 139/1, 44/1, 4874/100
    ...
```

### With Great Power ...

I needed this today because a photo was displayed in different orientations on different operating systems (correct on Windows, upside down on Ubuntu) and I wanted to find out what the photo actually says about it (spoiler: <span class="spoiler">of course Ubuntu was right :smile:</span>).

It showed me once again how easy it is to reveal personal information. A photo taken with my mobile phone at home and uploaded to Facebook? Then you don't need to leave an address - where you live is clear anyway.

Translated with www.DeepL.com/Translator (free version)