# Flutter Image Cropper

A Flutter plugin for Android, iOS and Web supports cropping images. This plugin is based on three different native libraries so it comes with different UI between these platforms.

## Introduction

Image Cropper doesn't manipulate images in Dart codes directly, instead, the plugin uses Platform Channel to expose Dart APIs that Flutter application can use to communicate with three very powerful native libraries (uCrop, TOCropViewController and croppie) to crop and rotate images. Because of that, all credits belong to these libraries.

### uCrop - Yalantis

This project aims to provide an ultimate and flexible image cropping experience. Made in Yalantis

### TOCropViewController - TimOliver

TOCropViewController is an open-source UIViewController subclass to crop out sections of UIImage objects, as well as perform basic rotations. It is excellent for things like editing profile pictures, or sharing parts of a photo online. It has been designed with the iOS Photos app editor in mind, and as such, behaves in a way that should already feel familiar to users of iOS.

### Croppie - Foliotek

Croppie is a fast, easy to use image cropping plugin with tons of configuration options!

## How to install

### Android

Add UCropActivity into your AndroidManifest.xml

   ```
   <activity
    android:name="com.yalantis.ucrop.UCropActivity"
    android:screenOrientation="portrait"
    android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>
   ```

### iOS

No configuration required

### Web

Add following codes inside <head> tag in file web/index.html:

   ```
   <head>
  ....

  <!-- Croppie -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/croppie/2.6.5/croppie.css" />
  <script defer src="https://cdnjs.cloudflare.com/ajax/libs/exif-js/2.3.0/exif.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/croppie/2.6.5/croppie.min.js"></script>

  ....
</head>
   ```

## Usage

### Required parameters

sourcePath: the absolute path of an image file.

### Optional parameters

1. maxWidth: maximum cropped image width. Note: this field is ignored on Web.

2. maxHeight: maximum cropped image height. Note: this field is ignored on Web.

3. aspectRatio: controls the aspect ratio of crop bounds. If this values is set, the cropper is locked and user can't change the aspect ratio of crop bounds. Note: this field is ignored on Web.

4. aspectRatioPresets: controls the list of aspect ratios in the crop menu view. In Android, you can set the initialized aspect ratio when starting the cropper by setting the value of AndroidUiSettings.initAspectRatio. Note: this field is ignored on Web.

5. cropStyle: controls the style of crop bounds, it can be rectangle or circle style (default is CropStyle.rectangle). Note: this field can be overrided by WebUiSettings.viewPort.type on Web

6. compressFormat: the format of result image, png or jpg (default is ImageCompressFormat.jpg).

7. compressQuality: number between 0 and 100 to control the quality of image compression.

8. uiSettings: controls UI customization on specific platform (android, ios, web,...)

   Android: see Android customization.

   iOS: see iOS customization.

   Web: see Web customization.

### Note

1. The result file is saved in NSTemporaryDirectory on iOS and application Cache directory on Android, so it can be lost later, you are responsible for storing it somewhere permanent (if needed).

2. The implementation on Web is much different compared to the implementation on mobile app. It causes some configuration fields not working (maxWidth, maxHeight, aspectRatio, aspectRatioPresets) on Web.

3. WebUiSettings is required for Web.

## Example

   ```
   import 'package:image_cropper/image_cropper.dart';

CroppedFile croppedFile = await ImageCropper().cropImage(
      sourcePath: imageFile.path,
      aspectRatioPresets: [
        CropAspectRatioPreset.square,
        CropAspectRatioPreset.ratio3x2,
        CropAspectRatioPreset.original,
        CropAspectRatioPreset.ratio4x3,
        CropAspectRatioPreset.ratio16x9
      ],
      uiSettings: [
        AndroidUiSettings(
            toolbarTitle: 'Cropper',
            toolbarColor: Colors.deepOrange,
            toolbarWidgetColor: Colors.white,
            initAspectRatio: CropAspectRatioPreset.original,
            lockAspectRatio: false),
        IOSUiSettings(
          title: 'Cropper',
        ),
        WebUiSettings(
          context: context,
        ),
      ],
    );
   ```

