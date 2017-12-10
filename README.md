# Canvas fingerprinting from scratch

## What is it?

Native JS library for fetching canvas fingerprinting  

## What is canvas fingerprinting?

Canvas fingerprinting is a techniques of tracking online users that allow websites to identify and track visitors using 
HTML5 canvas element instead of browser cookies or other similar means.

Main responsibility of canvas fingerprinting is collecting information about a remote computing device for the purpose 
of identification. Even if you are browsing GitHub now, this site try to fetch as much as possible your metadata like 
fonts, OS, specific system properties and it tries to draw hidden 3D [canvas](https://en.wikipedia.org/wiki/Canvas_element) 
which will be converted in unique token for  further identification on another sites, advertising some bullshit goods
on e-commerce sites or for recommendation system in social network.

Variation of GPU time rendering of 3D canvas allows to generate unique token for each user.
Also, this approach was widely used for deanonymization of Tor's user, in force of this fact, Tor's developers made 
a patch for this exploit, for now, Tor notifies user of canvas read attempt and return blank image data.

## How to protect yourself from it?

A lot of add-ons allow you to prevent attempt to fetch canvas fingerprint like 
[Privacy Badger](https://en.wikipedia.org/wiki/Privacy_Badger) 
or [Canvas Defender](https://chrome.google.com/webstore/detail/canvas-defender/obdbgnebcljmgkoljcdddaopadkifnpm?hl=en),
as mentioned above, Tor browser protect users from it by default. From October of 2017 Firefox [prevents canvas
fingerprinting](https://thehackernews.com/2017/10/canvas-browser-fingerprint-blocker.html) by default.

## Examples:
[Docker hub](https://hub.docker.com/) attempted to fetch canvas fingerprint, but was locked by add-on:


![Attempt](https://github.com/arukavytsia/canvas-fingerprinting/raw/master/images/2.png "Attempt to fetch fingerprint")


[Canvas Defender](https://chrome.google.com/webstore/detail/canvas-defender/obdbgnebcljmgkoljcdddaopadkifnpm?hl=en)
allows to generate dummy noise:

![Dummy noise](https://github.com/arukavytsia/canvas-fingerprinting/raw/master/images/3.png "Dummy fingerprint")

Noise generated by canvas defender:

![Dummy noise result](https://github.com/arukavytsia/canvas-fingerprinting/raw/master/images/1.png "Generated dummy fingerprint")


## How does it work?

This library uses multiple sources for generating unique token:

1. Canvas fingerprinting
2. Screen Resolution
3. Color Depth
4. Screen Resolution
5. Time Zone
6. UserAgent
7. Languages
8. CPU class
9. Fact of storing/presence/facts of:
    - session storage
    - local storage
    - indexed DB
    - open DB
    - IE `AddBehaviour`
    - IE specific plugins like `RealPlayer`, `AcroPDF`, `AgControl.AgControl - for Silverlight` etc.
    - [DNT header](https://en.wikipedia.org/wiki/Do_Not_Track) - as ironic as it was

After collection all metadata in a single place, they are joined with 
[MurmurHash](https://en.wikipedia.org/wiki/MurmurHash), in result, you have integer value which represents fingerprint
of your browser.
    
## Library usage:

```javascript
var fingerprint = new Fingerprint();
```

If you want to control drawing flag, just pass it directly `canvas: true` 
```javascript
    var withCanvasDrawing = new Fingerprint({canvas: true});
    var withoutCanvasDrawing = new Fingerprint({canvas: false});
```

### Using custom hashing function

```javascript
    var withCanvasDrawing = new Fingerprint({hasher: pearson});
   
```

## **License: MIT**
