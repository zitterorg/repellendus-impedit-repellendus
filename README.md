# @zitterorg/repellendus-impedit-repellendus

<a href="https://www.npmjs.com/package/@zitterorg/repellendus-impedit-repellendus"><img alt="npm version" src="https://badge.fury.io/js/@zitterorg/repellendus-impedit-repellendus.svg"></a>
<a href="https://npmjs.org/package/@zitterorg/repellendus-impedit-repellendus"><img alt="Downloads" src="http://img.shields.io/npm/dm/@zitterorg/repellendus-impedit-repellendus.svg"></a>
[![Build Status](https://travis-ci.org/mosch/@zitterorg/repellendus-impedit-repellendus.svg?branch=master)](https://travis-ci.org/mosch/@zitterorg/repellendus-impedit-repellendus)
[![Design](https://contribute.design/api/shield/mosch/@zitterorg/repellendus-impedit-repellendus)](https://contribute.design/mosch/@zitterorg/repellendus-impedit-repellendus)

Avatar / profile picture cropping component (like on Facebook). 
Resize, crop and rotate your uploaded image using a simple and clean user interface.

## Features

- Fully typed, written in TypeScript
- Provide your own input controls
- Resize
- Crop
- Rotate
- Rounded or square image result 

## Install

Just use your favorite package manager to add `@zitterorg/repellendus-impedit-repellendus` to your project:

```sh
yarn add @zitterorg/repellendus-impedit-repellendus

npm i --save @zitterorg/repellendus-impedit-repellendus

pnpm add @zitterorg/repellendus-impedit-repellendus
```

## [Demo](https://@zitterorg/repellendus-impedit-repellendus.netlify.com/)

![](https://thumbs.gfycat.com/FlawedBlushingGermanwirehairedpointer-size_restricted.gif)


## Usage

```javascript
import React from 'react'
import AvatarEditor from '@zitterorg/repellendus-impedit-repellendus'

const MyEditor = () => {
  return (
    <AvatarEditor
      image="http://example.com/initialimage.jpg"
      width={250}
      height={250}
      border={50}
      color={[255, 255, 255, 0.6]} // RGBA
      scale={1.2}
      rotate={0}
    />
  )
}

export default MyEditor
```

### Props

| Prop                   | Type             | Description                                                                                                                                                                                                                                                          |
| ---------------------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| image                  | String\|File     | The URL of the image to use, or a File (e.g. from a file input).                                                                                                                                                                                                     |
| width                  | Number           | The total width of the editor.                                                                                                                                                                                                                                       |
| height                 | Number           | The total height of the editor.                                                                                                                                                                                                                                      |
| border                 | Number\|Number[] | The cropping border. Image will be visible through the border, but cut off in the resulting image. Treated as horizontal and vertical borders when passed an array.                                                                                                  |
| borderRadius           | Number           | The cropping area border radius.                                                                                                                                                                                                                                     |
| color                  | Number[]         | The color of the cropping border, in the form: [red (0-255), green (0-255), blue (0-255), alpha (0.0-1.0)].                                                                                                                                                          |
| backgroundColor        | String           | The background color of the image if it's transparent.                                                                                                                                                                                                               |
| style                  | Object           | Styles for the canvas element.                                                                                                                                                                                                                                       |
| scale                  | Number           | The scale of the image. You can use this to add your own resizing slider.                                                                                                                                                                                            |
| position               | Object           | The x and y co-ordinates (in the range 0 to 1) of the center of the cropping area of the image. Note that if you set this prop, you will need to keep it up to date via onPositionChange in order for panning to continue working.                                   |
| rotate                 | Number           | The rotation degree of the image. You can use this to rotate image (e.g 90, 270 degrees).                                                                                                                                                                            |
| crossOrigin            | String           | The value to use for the crossOrigin property of the image, if loaded from a non-data URL. Valid values are `"anonymous"` and `"use-credentials"`. See [this page](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) for more information. |
| className              | String\|String[] | className property passed to the canvas element                                                                                                                                                                                                                      |
| onLoadFailure(event)   | function         | Invoked when an image (whether passed by props or dropped) load fails.                                                                                                                                                                                               |
| onLoadSuccess(imgInfo) | function         | Invoked when an image (whether passed by props or dropped) load succeeds.                                                                                                                                                                                            |
| onImageReady(event)    | function         | Invoked when the image is painted on the canvas the first time.                                                                                                                                                                                                      |
| onMouseUp()            | function         | Invoked when the user releases their mouse button after interacting with the editor.                                                                                                                                                                                 |
| onMouseMove(event)     | function         | Invoked when the user hold and moving the image.                                                                                                                                                                                                                     |
| onImageChange()        | function         | Invoked when the user changed the image. Not invoked on the first render, and invoked multiple times during drag, etc.                                                                                                                                               |
| onPositionChange()     | function         | Invoked when the user pans the editor to change the selected area of the image. Passed a position object in the form `{ x: 0.5, y: 0.5 }` where x and y are the relative x and y coordinates of the center of the selected area.                                     |
| disableBoundaryChecks  | Boolean          | Set to `true` to allow the image to be moved outside the cropping boundary.                                                                                                                                                                                          |
| disableHiDPIScaling    | Boolean          | Set to `true` to disable devicePixelRatio based canvas scaling. Can improve performance of very large canvases on mobile devices.                                                                                                                                    |

### Accessing the resulting image

The resulting image will have the same resolution as the original image, for that you can use `getImage`, regardless of the editor's size.
If you want the image sized in the dimensions of the canvas you can use `getImageScaledToCanvas`.

```javascript
import React from 'react'
import AvatarEditor from '@zitterorg/repellendus-impedit-repellendus'

const MyEditor = () => {
  const editor = useRef(null);

  render() {
    return (
      <div>
        <AvatarEditor
          ref={editor}
          image="http://example.com/initialimage.jpg"
          width={250}
          height={250}
          border={50}
          scale={1.2}
        />
        <button onClick={() => {
          if (this.editor) {
            // This returns a HTMLCanvasElement, it can be made into a data URL or a blob,
            // drawn on another canvas, or added to the DOM.
            const canvas = editor.current.getImage()

            // If you want the image resized to the canvas size (also a HTMLCanvasElement)
            const canvasScaled = editor.current.getImageScaledToCanvas()
          }
        }}>Save</button>
      </div>
    )
  }
}

export default MyEditor
```

### Adding drag and drop

We recommend using [react-dropzone](https://github.com/react-dropzone/react-dropzone). It allows you to add
drag and drop support to anything really easy. Here is an example how to use it with @zitterorg/repellendus-impedit-repellendus:

```javascript
import React, { useState } from 'react'
import AvatarEditor from '@zitterorg/repellendus-impedit-repellendus'
import Dropzone from 'react-dropzone'

const MyEditor = () => {
  const [image, setImage] = useState('http://example.com/initialimage.jpg')

  return (
    <Dropzone
      onDrop={(dropped) => setImage(dropped[0])}
      noClick
      noKeyboard
      style={{ width: '250px', height: '250px' }}
    >
      {({ getRootProps, getInputProps }) => (
        <div {...getRootProps()}>
          <AvatarEditor width={250} height={250} image={image} />
          <input {...getInputProps()} />
        </div>
      )}
    </Dropzone>
  )
}
```

### Accessing the cropping rectangle

Sometimes you will need to get the cropping rectangle (the coordinates of the area of the image to keep),
for example in case you intend to perform the actual cropping server-side.

`getCroppingRect()` returns an object with four properties: `x`, `y`, `width` and `height`;
all relative to the image size (that is, comprised between 0 and 1). It is a method of AvatarEditor elements,
like `getImage()`.

_Note that:_ `getImage()` returns a canvas element and if you want to use it in `src` attribute of `img`, convert it into a blob url.

```js
const getImageUrl = async () => {
  const dataUrl = editor.getImage().toDataURL()
  const res = await fetch(dataUrl)
  const blob = await res.blob()

  return window.URL.createObjectURL(blob)
}

// Usage
const imageURL = await getImageUrl()

<img src={imageURL} ... />
```

## Contributing

For development you can use following build tools:

- `npm run build`: Builds the _minified_ dist file: `dist/index.js`
- `npm run watch`: Watches for file changes and builds _unminified_ into: `dist/index.js`
- `npm run demo:build`: Builds the demo based on the dist file `dist/index.js`
- `npm run demo:watch`: Run webpack-dev-server. Check demo website [localhost:8080](http://localhost:8080)

### Kudos

Kudos and thanks to [dan-lee](https://github.com/dan-lee) for the work & many contributions to this project!
Also [oyeanuj](https://github.com/oyeanuj), [mtlewis](https://github.com/mtlewis) and [hu9o](https://github.com/hu9o) and all other awesome people contributing to this in any way.
