# file_picker_writable

Flutter plugin to choose files which can be read, referenced and written back at a
  later time (persistent permissions on android, secure bookmarks on iOS).


# Requirements

* Currently only supports 
    [plugin api v2](https://flutter.dev/docs/development/packages-and-plugins/plugin-api-migration).

## iOS

## Android

* Android 4.4 (API Level 4.4)

## Getting Started

See the example on how to implement it in a simple application.

```dart
Future<void> readFile() async {
  final fileInfo = await FilePickerWritable().openFilePicker();
  _logger.fine('Got picker result: $fileInfo');
  if (fileInfo == null) {
    _logger.info('User canceled.');
    return;
  }
  // now do something useful with the selected file...
  _logger.info('Got file contents in temporary file: ${fileInfo.file}');
  _logger.info('fileName: ${fileInfo.fileName}');
  _logger.info('Identifier which can be persisted for later retrieval:'
      '${fileInfo.identifier}');
}
```

The returned `fileInfo.identifier` can be used later to write or read from the data,
even after an app restart.

```dart
Future<void> persistChanges(FileInfo fileInfo, Uint8List newContent) async {
  // create a new temporary file inside your apps sandbox.
  final File f = _createNewTempFile();
  await f.writeBytes(newContent);

  // tell FilePickerWritable plugin to write the new contents over the user selected file
  await FilePickerWritable()
     .writeFileWithIdentifier(fileInfo.identifier, fi.file);
}
```
