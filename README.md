**This repo is now deprecated. Please use [airbnb/react-native-maps](https://github.com/airbnb/react-native-maps) instead.**

# React Native Mapping Integration

This package provides a React Native compatible, Google Maps UI component which runs on iOS and Android using the same
JavaScript API.

![screen shot](https://raw.githubusercontent.com/Pod-Point-Open-Source/react-native-maps/master/screen-shot.png "screen shot")

## Installation

```bash
npm install @pod-point-open-source/react-native-maps --save
```

## iOS Setup Guide

 1. Go get yourself a cup of coffee, this could take a while...
 2. Open up your React Native project in XCode, this is the `.xcodeproj` file in the `ios` directory of your React
 Native project.
 3. Click on the root of your project in XCode, then select your project's main target. Select *Build Settings* and then
 search for *Framework Search Paths*. Add 
 `$(PROJECT_DIR)/../node_modules/@pod-point-open-source/react-native-maps/ios_modules/GoogleMaps-1.11.1/Frameworks` to the framework
 search path list and make sure it is set to *recursive*.
 4. Now search for *Header Search Paths*. Add `$(SRCROOT)/../node_modules/@pod-point-open-source/react-native-maps` to the header
 search path list and make sure that it is also set to *recursive*.
 5. Open `node_modules/@pod-point-open-source/react-native-maps/ios` in Finder and locate the `PPTMapView.xcodeproj` package.
 Drag this file into the XCode project navigator. You can keep this in the `Libraries` group along with all the other
 React Native packages.
 6. Expand the `PPTMapView.xcodeproj` tree and select `GoogleMapsApi.plist` - drag this into the group which contains
 your `AppDelegate.h` and `AppDelegate.m` files; this group is usually named after your app. When prompted ensure that
 *Copy Items if Needed* is deselected when prompted, this will prevent this file from being committed into source 
 control. Open up the file and enter your Google API key into the value column of the row named `API Key`.
 7. Open up `AppDelegate.m` and add `#import "PPTGoogleMapProvider.h"` at the top of the file. Then add 
 `[PPTGoogleMapProvider provideAPIKey];` somewhere in the `application` method, ideally at the top.
 8. Select the `Google Maps SDK` group in `PPTMapView.xcodeproj`,  drag these packages  into the `Libraries` group of 
 your React Native project and ensure that *Copy Items if Needed* is deselected when prompted.
 9. Click on the root of your project in XCode, then select your project's main target. Click on *Build Phases* and
 double check that all the libraries and frameworks were automatically added to the *Link Binary With Libraries* phase.
 If they weren't, select all the packages in the Google Maps SDK group (apart from `GoogleMaps.bundle`) and drag them
 into this build phase.
 10. At the bottom of the *Link Binary With Libraries* list, click the `+` button and search for `libPPTMapView.a` (it should be in the `Workspace` folder). Select `libPPTMapView.a` and click the `Add` button. Scroll back up to the top of the list and double check that it was added.
 11. Hit `Cmd+R` and make sure the app runs!
 
## Android Setup Guide
 
 1. Open up your React Native project in Android Studio, this is the `android` directory in your React Native project.
 2. Expand *Gradle Scripts* from within the project tree and open `settings.gradle`. Replace the line in the script
 which states `include ':app'` with `include ':app', ':pptmapview'` (or append `':pptmapview'` to the end of the
 include statement if you're already including other modules).
 3. Add the following line to the end of `settings.gradle`:
 ```
 project(':pptmapview').projectDir = new File(rootProject.projectDir, '../node_modules/@pod-point-open-source/react-native-maps/android/library')
 ```
 4. Open up your `app` module `build.gradle` file and add the following line to the end of your dependancies section:
 ```
 compile project(path: ':pptmapview')
 ```
 5. You should now be prompted to run a Gradle project sync so press *Sync Now* in the gold toolbar that should be
 visible.
 6. Open your projects `MainActivity` class and import the following package:
 ```java
 import com.podpoint.pptmapview.PPTGoogleMapPackage;
 ```
 7. Find the line in your main activity class which has the following on it - `.addPackage(new MainReactPackage())`,
 add the following line below:
 ```java
 .addPackage(new PPTGoogleMapPackage())
 ```
 8. Expand the `pptmapview` package in your project explorer and then expand the `manifests` directory. Open up the
 `AndroidManifset.xml` and find the node with the key `com.google.android.geo.API_KEY`. Enter your Google API key
 into the `android:value` property and save the file. This file will be kept out of source control so it is safe to
 store the API key in here.
 9. Hit `Ctrl+R` and make sure the app runs!

## Creating a new Map

Require the UI component in the component you're wanting to display a map in:

```javascript
import { GoogleMap } from '@pod-point-open-source/react-native-maps';
```

Include the following JSX in your render method:

```javascript
<GoogleMap />
```

You will need to style the component appropriately so that it is visible, just like any other React
Native view component.

## Map Options

There are a number of options for the map view which let you customise its layout and UI options. These
are specified as JSX parameters like so:

```javascript
<GoogleMap
    cameraPosition={{auto: true, zoom: 10}}
    showsUserLocation={true}
    scrollGestures={true}
    zoomGestures={true}
    tiltGestures={true}
    rotateGestures={true}
    consumesGesturesInView={true}
    compassButton={true}
    myLocationButton={true}
    indoorPicker={true}
    allowScrollGesturesDuringRotateOrZoom={true}
/>
```

 * `cameraPosition` - The map view camera position. You can set `auto` to true and specify a zoom level
    or you can pass the latitude, longitude and zoom level to manually position the camera.
 * `showsUserLocation` - If true the app will ask for the user's location and focus on it. Default value is false.
 * `scrollGestures` - Controls whether scroll gestures are enabled (default) or disabled.
 * `zoomGestures` - Controls whether zoom gestures are enabled (default) or disabled.
 * `tiltGestures` - Controls whether tilt gestures are enabled (default) or disabled.
 * `rotateGestures` - Controls whether rotate gestures are enabled (default) or disabled.
 * `consumesGesturesInView` - Controls whether gestures by users are completely consumed by the GMSMapView when gestures
   are enabled (default YES).
 * `compassButton` - Enables or disables the compass.
 * `myLocationButton` - Enables or disables the My Location button.
 * `indoorPicker` - Enables (default) or disables the indoor floor picker.
 * `allowScrollGesturesDuringRotateOrZoom` - Controls whether rotate and zoom gestures can be performed off-center and 
   scrolled around (default YES).
   
## Map Markers

Map markers can be set by passing an array of objects which describe them. The markers can be the stock Google variety,
or you can pass a reference to an image to customise them. The stock Google marker color can be customized by setting
the `hexColor` property (7 characters, including a `#` prefix). It's also possible to add metadata to the marker, simply add
a `meta` parameter to the marker object. All markers require a unique identifier string, these should be formatted in a
similar way to an id tag in HTML. This metadata is returned when an event which affects the marker is fired such
as `didTapMarker`. Markers are specified using a JSX parameter:

```javascript
<GoogleMap
    markers={[
        {
            id: 'marker-100',
            latitude: 51.5072,
            longitude: -0.1275
        },
        {
            id: 'marker-101',
            latitude: 53.2031,
            longitude: -1.5621,
            icon: require('./images/my-custom-marker.png')
        },
        {
            id: 'marker-102',
            latitude: 21.7342,
            longitude: -5.7350,
            meta: {
                foo: 'bar'
            }
        },
        {
            id: 'marker-103',
            latitude: 56.2031,
            longitude: -1.7621,
            hexColor: '#00abff'
        },
    ]}
/>
```

## Map Events

Event listeners can be attached to the map in the form of a callback. These are specified as a JSX parameter like so:

```javascript
<GoogleMap
    didTapMarker:{function(event) {
        console.log(event.name);
    }}
/>
```

The following events listeners are available:

 * `didChangeCameraPosition` - Called repeatedly during any animations or gestures on the map (or once, if the camera 
    is explicitly set). This may not be called for all intermediate camera positions. It is always called for the final 
    position of an animation or gesture - _iOS and Android_.
 * `idleAtCameraPosition` - Called when the map becomes idle, after any outstanding gestures or animations have 
    completed (or after the camera has been explicitly set) - _iOS Only_.
 * `didTapAtCoordinate` - Called after a tap gesture at a particular coordinate, but only if a marker was not tapped
    - _iOS and Android_.
    This is called before deselecting any currently selected marker (the implicit action for tapping on the map).
 * `didLongPressAtCoordinate` - Called after a long-press gesture at a particular coordinate - _iOS and Android_.
 * `didTapMarker` - Called after a marker has been tapped - _iOS and Android_.
 * `didTapOverlay` - Called after an overlay has been tapped - _iOS Only_.
 * `didBeginDraggingMarker` - Called when dragging has been initiated on a marker - _iOS and Android_.
 * `didEndDraggingMarker` - Called after dragging of a marker ended - _iOS and Android_.
 * `didDragMarker` - Called while a marker is dragged - _iOS and Android_.
 * `didTapMyLocationButtonForMapView` - Called when the My Location button is tapped - _iOS and Android_.
 
An object is returned with details about the event, these typically look like:

```javascript
{
    id: 'marker-102',
    name: "didTapMarker",
    data: { 
        latitude: 21.7342,
        longitude: -5.7350,
        meta: {
            foo: 'bar'
        }
    }
}
```

## Contributing

Thank you for considering contributing to this package! The contribution guide can be found [here](http://www.contribution-guide.org/).

## Security Vulnerabilities

If you discover a security vulnerability within this package, please send an e-mail to software@pod-point.com. All security vulnerabilities will be promptly addressed.
