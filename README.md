nodert-streams
=====
Turns a WinRT stream (Wrapped using NodeRT) into a node.js stream.
For more information on NodeRT, take a look at: <a href="https://github.com/NodeRT/NodeRT" target="_blank">https://github.com/NodeRT/NodeRT</a>

You can download this module from NPM by running the following cmd:

```
npm install @nodert-win10/nodert-streams
```

If you have cloned the git repo (and not downloaded thm module from NPM) run the following command from inside the directory:

```
node-gyp rebuild
```

(For both of the above commands, make sure that <a href="https://github.com/TooTallNate/node-gyp" target="_blank">node-gyp</a> and all of its perquisites are installed)

<h3>API:</h3>

* **InputStream** class - instantiated by passing a WinRT input stream in the constructor, this object behaves like a readable nodejs stream.
* **OutputStream** class - instantiated by passing a WinRT input stream in the constructor, this object behaves like a writeable nodejs stream.
* **toIBuffer** function - accepts a node.js buffer and returns an IBuffer object which points to the same underlying native buffer.

Here is an example of using nodert-streams for piping contents of a file to stdout:

```javascript
var streams = require('@nodert-win10/windows.storage.streams');
var storage = require('@nodert-win10/windows.storage');
var nodert_streams = require('@nodert-win10/nodert-streams');
var StorageFile = storage.StorageFile;

// open a file from the documents library
storage.KnownFolders.documentsLibrary.getFileAsync('document.txt', function(err, file) {
  if (err) {
    return console.info(err);
  }
  // call openAsync in order to retrieve the stream
  file.openAsync(storage.FileAccessMode.read, function(err, fileStream) {
    if (err) {
      return console.info(err);
    }
    // convert the winrt stream to a node.js stream
    var nodeStream = streams.InputStream(fileStream.getInputStreamAt(0));
    // pipe the stream to the stdout
    nodeStream.pipe(process.stdout);
  });
});
```
