# DraftJS Drag & Drop File Upload Plugin with Custom Image Upload Option

*This is a plugin for the `draft-js-plugins-editor`.*

This plugin adds dropping & uploading files functionality to your editor with uploading image to server and using image URL instead of base64 format

Usage:

```js
import createDragNDropUploadPlugin from '@ajmalpkc/draft-js-drag-n-drop-upload-plugin';

import upload from './upload';
...
...

export const imagePlugin = createImagePlugin({ decorator });

export const dragNDropFileUploadPlugin = createDragNDropUploadPlugin({
  handleUpload: upload, //write your image upload codes in upload.js
  addImage: imagePlugin.addImage,
});

const dndFileUploadPlugin = createDndFileUploadPlugin();
```



upload.js

```js
// Read file contents intelligently as plain text/json, image as dataUrl or
// anything else as binary

function readFile(file) {
    return new Promise(function (resolve) {
        var reader = new FileReader();

        // This is called when finished reading
        reader.onload = function (event) {
            // Return an array with one image
            if (file.type.indexOf("image/") === 0) {
                const src = `YOUR IMAGE URL`;

                resolve({
                    // These are attributes like size, name, type, ...
                    lastModifiedDate: file.lastModifiedDate,
                    lastModified: file.lastModified,
                    name: file.name,
                    size: file.size,
                    type: file.type,

                    // This is the files content as base64
                    src: src //event.target.result
                });
            } else {
                resolve({
                    // These are attributes like size, name, type, ...
                    lastModifiedDate: file.lastModifiedDate,
                    lastModified: file.lastModified,
                    name: file.name,
                    size: file.size,
                    type: file.type,

                    // This is the files content as base64
                    src: event.target.result
                });
            }

        };

        if (file.type.indexOf('text/') === 0 || file.type === 'application/json') {
            reader.readAsText(file);
        } else if (file.type.indexOf('image/') === 0) {
            reader.readAsDataURL(file);
        } else {
            reader.readAsBinaryString(file);
        }
    });
}

// Read multiple files using above function
export default function readFiles(files) {
    return Promise.all(files.map(readFile));
}

```
