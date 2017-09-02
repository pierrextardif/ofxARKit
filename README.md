# ofxARKit
openFrameworks + ARKit


## A Basic toolkit for an IOS app with ARKit support.
This is an openFrameworks addon that provides some helper classes for working within ARKit.

# Installation
* Download openFrameworks [here](http://openframeworks.cc/versions/v0.9.8/of_v0.9.8_ios_release.zip)
* clone this repo into your addons folder 

# Current functionality 
ARKit, while it does do a ton behind the scenes; it pretty much leaves it up to you to figure out how you want to render things. The current Apple documentation, while already moderately detailed, unfortunately leaves some stuff out. 

There are a number of classes and other files in the addon that deal with different areas relating to ARKit, like setting up the camera, dealing with feature detection, or dealing with plane detection.

The class `ARProcessor` deals with joining all of these different bits of functionality in a (hopefully) easy to use API, but each of the classes can be used as standalone classes as well.

There are the following classes/files that are part of the addon
* `ARAnchorManager` : deals with managing `ARAnchor` objects as well as `ARPlaneAnchor` objects.
* `ARCam` : deals with managing the camera data found by ARKit and generating something that can be displayed.
* `ARDebugUtils` : as the name suggests - this deals with debugging helpers. At the moment, it's able to handle feature detection and drawing a point cloud.
* `ARObject` : this is a header file that declares `PlaneAnchorObject` and `ARObject`. These structs are used to store converted ARKit data into something more oF friendly.
* `ARShaders` : this stores the core shaders needed by the addon.
* `ARUtils.h` : this stores various utility functions

Note that if you've used the addon pior to 8/29/2017, though I did my best to not make any api changes, there is a very tiny chance your code may break.

# Setting up ARKit
ARKit actually does a ton of things in and of itself. All you really have to do is initialize the framework in your ViewController, then you manipulate that session object to grab information the framework provides. 

The goal with how this is setup is to be as flexible as possible so this can be integrated into a wide variety of situations. 
Also because it's really only a few lines to initialize ARKit. 

__First__
* [get xcode beta](https://developer.apple.com/download/)
* [get ios 11 beta](https://beta.apple.com/sp/betaprogram/guide)
* In your project, remember to edit your `ofxIOS-Info.plist` file to include the `Privacy - Camera Usage Description` key with a description for the alert dialog that pops up asking the user for use of their camera.

To initialize the ARKit framework
```objective-c
    @interface <your view controller name>()
      @property (nonatomic, strong) ARSession *session;
    @end


    // then somewhere in your implementation block...
    // official example shows you ought to declare the session in viewWillLoad and initialize in viewWillAppear, but 
    // that can result in tracking performance degradation in my experience (again, could just be the phone I'm borrowing, 
    // your milage may vary ¯\_(ツ)_/¯)

   self.session = [ARSession new];
    
    ARWorldTrackingConfiguration *configuration = [ARWorldTrackingConfiguration new];

    // setup horizontal plane detection
    configuration.planeDetection = ARPlaneDetectionHorizontal;
    
    [self.session runWithConfiguration:configuration];
```
note that - assuming you're using objective c++(which should be the default for ios oF projects), you could just as easily skip adding a 
view controller and just initialize in ofApp.h/.mm

Lastly - include `ofxARKit.h`, this will give you access to all of the class files as well as the `ARProcessor` api class.

# Permissions
For ARKit - You'll have to enable the `Privacy - Camera Usage Description` in your `ofxiOS-Info.plist` file. This value for this field is just the string you want to show users when you ask for Camera permissions.


### Potential Hurdles in setup of ARKit
A strange occurance I've run into fairly often, and it's unclear as to why this happens; but it seems that, depending on where you initialize ARKit, that could potentially affect performance. ARKit will output console messages when there are issues, specifically in this case you may see as message along the lines of `...tracking performance reduced due to resource constraints...` In all likelihood it's simply hardware related as I don't have access to the latest iDevices but I have had situations where things have run well without ever seeing that message. 

That all being said, fps seems to be minimally affected if at all, though of course, as the message suggests, tracking ability might not be as good. Just something to be on the lookout for.


# Contributing
As I certainly am not the most knowledgeable on many of the topics required to work in AR, that and with ARKit still being in beta; if there's something you feel you can contribute, by all means, feel free to make PR's!

As long as it doesn't break anything I'll most likely accept it. Please make all PRs against the `develop` branch

A big thank you to all contributors thus far!
