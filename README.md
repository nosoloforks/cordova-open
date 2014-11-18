open
====

Open documents with compatible applications installed on the user's device.

![Open](https://raw.githubusercontent.com/cordova-bridge/open/docs/open.png =300x "Open")

## Install

```bash
$ cordova plugin add com.bridge.open
```

## Usage

The plugin exposes the following methods:

```javascript
cordova.plugins.bridge.open(file, success, error)
```

#### Parameters:

* __file:__ A string representing a local URI
* __success:__ Optional success callback
* __error:__ Optional error callback

## Example

#### Default usage

```javascript
cordova.plugins.bridge.open('file:/storage/sdcard/DCIM/Camera/1404177327783.jpg');
```

#### With optional callbacks

```javascript
var open = cordova.plugins.bridge.open;

function success() {
  console.log('Success');
}

function error(code) {
  if (code === 1) {
    console.log('No file handler found');
  } else {
    console.log('Undefined error');
  }
}

open('file:/storage/sdcard/DCIM/Camera/1404177327783.jpg', success, error);
```

#### Handling remote files

The plugin does not natively support remote files directly, but can be paired with `org.apache.cordova.file-transfer`:

```javascript
var ft = new FileTransfer(),
    url = fileUrl,
    filename = url.substring(url.lastIndexOf('/') + 1),
    uri = encodeURI(url),
    dir = (cordova.file.tempDirectory) ? 'tempDirectory' : 'externalCacheDirectory', // ios : android
    path = cordova.file[dir] + filename;

ft.download(uri, path,
    function done(entry) {
      cordova.plugins.bridge.open(entry.toURL(), success, error);
    },
    function fail(error) {
      console.log('download error', error);
    },
    false
);
```