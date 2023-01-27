# quick-sprite

Small utility to quicky generate sprites from multiple images.

There are a lot of options for building Sprites! Use this library if you want a non-opinionated solution that is easy to call programaticaly in a `node` context (a lot of other libraries assume build pipelines or access to Browser types). 

You may also like this library if you are already using `Jimp` to handle image processing.

### usage

Install package

```
npm install quick-sprite --save
```

Example:

```ts
import {createSprite} from 'quick-sprite';

const sources: ImageSource[] = [
  {key: 'image1', path: '<file-path-1>'},
  {key: 'image2', path: '<file-path-2>'},
  {key: 'image3', path: '<file-path-3>'},
];

createSprite(sources).then(({image, mapping}: Sprite) => {
  // do something with `mapping`
  image.write('<output-path>');
});
```

Types

```ts
function createSprite(sources: ImageSource[], partialOptions: Partial<Options>): Promise<Sprite>

type ImageSource 
  = {key: string, path: string} 
  | {key: string, image: Jimp} 
  | {key: string, buffer: Buffer};

enum FillMode {
  Vertical = 'vertical',
  Horizontal = 'horizontal', 
  Row = 'row'
}

type Options = {
  fillMode: FillMode;
  maxWidth: number;
  dedupe: boolean;
  padding: number;
  transform: (key: string, image: Jimp) => Jimp,
}

type Sprite = {
  mapping: {
    [key: string]: {
      x: number,
      y: number,
      width: number,
      height: number,
    }
  },
  image: Jimp,
}
```

_Note: the only external dependency for this library is [`Jimp`](https://www.npmjs.com/package/jimp), which in turn has zero native dependencies. Most image processing calls are passed through to `Jimp`, and the final return type includes a `Jimp` instance._

### development

Install deps

```
npm install
```

Compile and typecheck

```
npm run build

// or

npm run watch
```

Run test

```
npm run test
```
