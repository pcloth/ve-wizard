# ve-wizard

[![npm](https://img.shields.io/npm/v/ve-wizard.svg) ![npm](https://img.shields.io/npm/dm/ve-wizard.svg)](https://www.npmjs.com/package/ve-wizard)
[![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)

This is an easy help wizard component

## Table of contents

- [Installation](#installation)
- [Usage](#usage)
- [Example](#example)

# Installation

```
npm install --save ve-wizard
```

## Default import

Install all the components:

```javascript
import Vue from 'vue'
import veWizard from 've-wizard'

Vue.use(veWizard)
```

Use specific components:

```javascript
import Vue from 'vue'
import { Test } from 've-wizard'

Vue.component('test', Test)
```

**⚠️ A css file is included when importing the package. You may have to setup your bundler to embed the css in your page.**

## Distribution import

Install all the components:

```javascript
import 've-wizard/dist/ve-wizard.css'
import veWizard from 've-wizard/dist/ve-wizard'

Vue.use(veWizard)
```

Use specific components:

```javascript
import 've-wizard/dist/ve-wizard.css'
import { Test } from 've-wizard/dist/ve-wizard'

Vue.component('test', Test)
```

**⚠️ You may have to setup your bundler to embed the css file in your page.**


The plugin should be auto-installed. If not, you can install it manually with the instructions below.

Install all the components:

```javascript
Vue.use(veWizard)
```

Use specific components:

```javascript
Vue.component('test', veWizard.Test)
```

## Source import

Install all the components:

```javascript
import Vue from 'vue'
import veWizard from 've-wizard/src'

Vue.use(veWizard)
```

Use specific components:

```javascript
import Vue from 'vue'
import { Test } from 've-wizard/src'

Vue.component('test', Test)
```

**⚠️ You need to configure your bundler to compile `.vue` files.** More info [in the official documentation](https://vuejs.org/v2/guide/single-file-components.html).

# Usage

> TODO

# Example

> TODO

---

# Plugin Development

## Installation

The first time you create or clone your plugin, you need to install the default dependencies:

```
npm install
```

## Watch and compile

This will run webpack in watching mode and output the compiled files in the `dist` folder.

```
npm run dev
```

## Manual build

This will build the plugin into the `dist` folder in production mode.

```
npm run build
```

---

## License

[MIT](http://opensource.org/licenses/MIT)
