# create-react-app

学习React的同学们，还在执拗于如何快速使用React进行开发么。现在有了一个帅气的工具帮助我们，接下来一起玩耍吧。


## 目录说明

```
my-react-app/
  README.md
  index.html
  favicon.ico
  node_modules/
  package.json
  src/
    App.css
    App.js
    index.css
    index.js
    logo.svg
```

## 如何开发


```
$ npm install
$ npm start
```

```
$ npm run build
$ cd build
$ npm install -g http-server
$ hs
```

``
$ npm run eject
```

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

## 如何开发

### Import a Component

This project setup supports ES6 modules thanks to Babel.
While you can still use `require()` and `module.exports`, we encourage you to use [`import` and `export`](http://exploringjs.com/es6/ch_modules.html) instead.

For example:

### `Button.js`

```js
import React, { Component } from 'react';

class Button extends Component {
  render() {
    // ...
  }
}

export default Button; // Don’t forget to use export default!
```

### `DangerButton.js`

```js
import React, { Component } from 'react';
import Button from './Button'; // Import a component from another file

class DangerButton extends Component {
  render() {
    return <Button color='red' />;
  }
}

export default DangerButton;
```

Be aware of the [difference between default and named exports](http://stackoverflow.com/questions/36795819/react-native-es-6-when-should-i-use-curly-braces-for-import/36796281#36796281). It is a common source of mistakes.

We suggest that you stick to using default imports and exports when a module only exports a single thing (for example, a component). That’s what you get when you use `export default Button` and `import Button from './Button'`.

Named exports are useful for utility modules that export several functions. A module may have at most one default export and as many named exports as you like.

Learn more about ES6 modules:

* [When to use the curly braces?](http://stackoverflow.com/questions/36795819/react-native-es-6-when-should-i-use-curly-braces-for-import/36796281#36796281)
* [Exploring ES6: Modules](http://exploringjs.com/es6/ch_modules.html)
* [Understanding ES6: Modules](https://leanpub.com/understandinges6/read#leanpub-auto-encapsulating-code-with-modules)

### Add a Stylesheet

This project setup uses [Webpack](https://webpack.github.io/) for handling all assets. Webpack offers a custom way of “extending” the concept of `import` beyond JavaScript. To express that a JavaScript file depends on a CSS file, you need to **import the CSS from the JavaScript file**:

#### `Button.css`

```css
.Button {
  padding: 20px;
}
```

#### `Button.js`

```js
import React, { Component } from 'react';
import './Button.css'; // Tell Webpack that Button.js uses these styles

class Button extends Component {
  render() {
    // You can use them as regular CSS styles
    return <div className='Button' />;
  }
}
```

**This is not required for React** but many people find this feature convenient. You can read about the benefits of this approach [here](https://medium.com/seek-ui-engineering/block-element-modifying-your-javascript-components-d7f99fcab52b). However you should be aware that this makes your code less portable to other build tools and environments than Webpack.

In development, expressing dependencies this way allows your styles to be reloaded on the fly as you edit them. In production, all CSS files will be concatenated into a single minified `.css` file in the build output.

If you are concerned about using Webpack-specific semantics, you can put all your CSS right into `src/index.css`. It would still be imported from `src/index.js`, but you could always remove that import if you later migrate to a different build tool.

### Post-Process CSS

This project setup minifies your CSS and adds vendor prefixes to it automatically through [Autoprefixer](https://github.com/postcss/autoprefixer) so you don’t need to worry about it.

For example, this:

```css
.App {
  display: flex;
  flex-direction: row;
  align-items: center;
}
```

becomes this:

```css
.App {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
}
```

There is currently no support for preprocessors such as Less, or for sharing variables across CSS files.

### Add Images and Fonts

With Webpack, using static assets like images and fonts works similarly to CSS.

You can **`import` an image right in a JavaScript module**. This tells Webpack to include that image in the bundle. Unlike CSS imports, importing an image or a font gives you a string value. This value is the final image path you can reference in your code.

Here is an example:

```js
import React from 'react';
import logo from './logo.png'; // Tell Webpack this JS file uses this image

console.log(logo); // /84287d09b8053c6fa598893b8910786a.png

function Header() {
  // Import result is the URL of your image
  return <img src={logo} alt="Logo" />;
}

export default function Header;
```

This works in CSS too:

```css
.Logo {
  background-image: url(./logo.png);
}
```

Webpack finds all relative module references in CSS (they start with `./`) and replaces them with the final paths from the compiled bundle. If you make a typo or accidentally delete an important file, you will see a compilation error, just like when you import a non-existent JavaScript module. The final filenames in the compiled bundle are generated by Webpack from content hashes. If the file content changes in the future, Webpack will give it a different name in production so you don’t need to worry about long-term caching of assets.

Please be advised that this is also a custom feature of Webpack.

**It is not required for React** but many people enjoy it (and React Native uses a similar mechanism for images). However it may not be portable to some other environments, such as Node.js and Browserify. If you prefer to reference static assets in a more traditional way outside the module system, please let us know [in this issue](https://github.com/facebookincubator/create-react-app/issues/28), and we will consider support for this.

### Adding Flow

Flow typing is currently [not supported out of the box](https://github.com/facebookincubator/create-react-app/issues/72) with the default `.flowconfig` generated by Flow. If you run it, you might get errors like this:

```
node_modules/fbjs/lib/Deferred.js.flow:60
 60:     Promise.prototype.done.apply(this._promise, arguments);
                           ^^^^ property `done`. Property not found in
495: declare class Promise<+R> {
     ^ Promise. See lib: /private/tmp/flow/flowlib_34952d31/core.js:495

node_modules/fbjs/lib/shallowEqual.js.flow:29
 29:     return x !== 0 || 1 / (x: $FlowIssue) === 1 / (y: $FlowIssue);
                                   ^^^^^^^^^^ identifier `$FlowIssue`. Could not resolve name

src/App.js:3
  3: import logo from './logo.svg';
                      ^^^^^^^^^^^^ ./logo.svg. Required module not found

src/App.js:4
  4: import './App.css';
            ^^^^^^^^^^^ ./App.css. Required module not found

src/index.js:5
  5: import './index.css';
            ^^^^^^^^^^^^^ ./index.css. Required module not found
```

To fix this, change your `.flowconfig` to look like this:

```
[libs]
./node_modules/fbjs/flow/lib

[options]
esproposal.class_static_fields=enable
esproposal.class_instance_fields=enable

module.name_mapper='^\(.*\)\.css$' -> 'react-scripts/config/flow/css'
module.name_mapper='^\(.*\)\.\(jpg\|png\|gif\|eot\|svg\|ttf\|woff\|woff2\|mp4\|webm\)$' -> 'react-scripts/config/flow/file'

suppress_type=$FlowIssue
suppress_type=$FlowFixMe
```

Re-run flow, and you sholdn’t get any extra issues.

If you later `eject`, you’ll need to replace `react-scripts` references with the `<PROJECT_ROOT>` placeholder, for example:

```
module.name_mapper='^\(.*\)\.css$' -> '<PROJECT_ROOT>/config/flow/css'
module.name_mapper='^\(.*\)\.\(jpg\|png\|gif\|eot\|svg\|ttf\|woff\|woff2\|mp4\|webm\)$' -> '<PROJECT_ROOT>/config/flow/file'
```

We will consider integrating more tightly with Flow in the future so that you don’t have to do this.

### Something Missing?

If you have ideas for more “How To” recipes that should be on this page, [let us know](https://github.com/facebookincubator/create-react-app/issues) or [contribute some!](https://github.com/facebookincubator/create-react-app/edit/master/template/README.md)
