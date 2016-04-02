# react-svg-loader

[![Build Status](https://travis-ci.org/boopathi/react-svg-loader.svg)](https://travis-ci.org/boopathi/react-svg-loader)

## Old version

Now `master` represents the new rewrite of the react-svg loader. Though this gives the exact same output as of the previous one, the entire thing is changed. So if you'd like to continue using the old one, it's in [`v0.1` branch](https://github.com/boopathi/react-svg-loader/tree/v0.1) and `~0.1.0` on npm

## Install

```sh
npm i react-svg-loader
```

## Usage

### with babel-loader

This outputs ES2015 and JSX code and should be transpiled with babel or any other transpiler that supports ES2015 and JSX.

```js
// In your webpack config
{
  test: /\.svg$/,
  loader: 'babel!react-svg'
}
```

### without babel-loader

Pass loader query `es5=true`.

Note: babel transform is applied with `react` and `es2015-loose` presets.

```js
// In your webpack config
{
  test: /\.svg$/,
  loader: 'react-svg?es5=1'
}
```

### Props to the output component pass through to the root svg element

```js
import Image from './arrow.svg';
// width and height will be passed to svg
<Image width={100} height={100} /> // <svg width=100 height=100>
```

### Props to the output component override the root svg element's prop

```js
// input: arrow.svg
// <svg width="16">
import Image from './arrow.svg';
<Image width={32}/> // <svg width="32">
```

## Internals

<p align="center">
Input SVG
</p>
<p align="center">↓</p>
<p align="center">
SVG Optimize using <a href="https://github.com/svg/svgo">SVGO</a>
</p>
<p align="center">↓</p>
<p align="center">
Babel Transform with `preset=react` and <a href="src/plugin.js">plugin=svgToComponent</a>
</p>

#### Input svg

```html
<svg width="50" namespaced:attr="unnecessary"/>
```

#### SVG Optimize

```html
<svg width="50"/>
```

#### Babel transform

```js
import React from 'react';
export default class SVG extends React.Component {
  render() {
    return <svg width={this.props.width ? this.props.width : "50"} {...this.props} />;
  }
}
```


## Options

The ouput svg component takes in options that are defined in the svg

## CLI

The react-svg-loader comes with a cli (`svg2react`) that you can use to convert svg files to react components. Use this tool when you'd want to customize your svg component by hand. Otherwise the loader just works.

```sh
`npm bin`/svg2react file1.svg file2.svg
```

and the following files will be emitted

+ `file1.svg.react.js`
+ `file2.svg.react.js`

in the SAME directory as the files

### CLI Options

`--es5`: Transforms ES2015+JSX output to ES5 using `presets=[es2015-loose, react]`

```
`npm bin`/svg2react file1.svg --es5
```

## Assumptions and Other gotchas

+ Root element is always `<svg>`
+ SVG is optimized

## LICENSE

MIT License - http://boopathi.mit-license.org
