---
permalink: 'demoing/index.html'
title: Demoing
section: guides
tags:
  - guides
---

# Demoing via storybook

For demoing, documenting and showcasing different states of your Web Component, we recommend using [storybook](https://storybook.js.org/).

⚠️ This repo is a fork of [open-wc's demoing-storybook package](https://github.com/open-wc/open-wc/packages/demoing-storybook)

Differences with this repo lies in :
- Current repo contains only scripts required to make a standalone storybook working
- It improves things, allowing to serve storybook from an http directory (typically useful when working behind a proxy)


[//]: # 'AUTO INSERT HEADER PREPUBLISH'

# Features

- Create API documentation/playground
- Use Storybook docs mode to showcase your elements within the normal text flow
- Works down to IE11
- Prebuilt storybook UI (for a fast startup)
- Uses es-dev-server (serve modern code while developing)
- Completely separate storybook UI from your code

## Demo

<div class="custom-block tip"><p class="custom-block-title">TIP</p> <p>Don't take our word for it but look at <a href="https://open-wc.org/demoing-storybook/?path=/docs/demo-card-docs--simple" target="_blank" rel="noopener noreferrer">the documentation of a demo card<svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" x="0px" y="0px" viewBox="0 0 100 100" width="15" height="15" class="icon outbound"><path fill="currentColor" d="M18.8,85.1h56l0,0c2.2,0,4-1.8,4-4v-32h-8v28h-48v-48h28v-8h-32l0,0c-2.2,0-4,1.8-4,4v56C14.8,83.3,16.6,85.1,18.8,85.1z"></path> <polygon fill="currentColor" points="45.7,48.7 51.3,54.3 77.2,28.5 77.2,37.2 85.2,37.2 85.2,14.9 62.8,14.9 62.8,22.9 71.5,22.9"></polygon></svg></a> and <a href="https://open-wc.org/demoing-storybook/?path=/docs/decorators-withwebcomponentknobs--example-output" target="_blank" rel="noopener noreferrer">the documentation of the knobs decorator<svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" x="0px" y="0px" viewBox="0 0 100 100" width="15" height="15" class="icon outbound"><path fill="currentColor" d="M18.8,85.1h56l0,0c2.2,0,4-1.8,4-4v-32h-8v28h-48v-48h28v-8h-32l0,0c-2.2,0-4,1.8-4,4v56C14.8,83.3,16.6,85.1,18.8,85.1z"></path> <polygon fill="currentColor" points="45.7,48.7 51.3,54.3 77.2,28.5 77.2,37.2 85.2,37.2 85.2,14.9 62.8,14.9 62.8,22.9 71.5,22.9"></polygon></svg></a>.</p></div>

## Setup

```bash
npm init @open-wc
# Upgrade > Demoing
```

### Manual

- `npm add fcr-storybook-prebuilt-wrapper --save-dev`
- Copy at minimum the [.storybook](https://github.com/fcamblor/fcr-storybook-prebuilt-wrapper/tree/master/demo/.storybook) folder to `.storybook`
- If you want to bring along the examples, you may also copy the `stories` folder.
- Be sure you have a [custom-elements.json](#custom-elementsjson) file.
- Add the following scripts to your package.json

```json
"scripts": {
  "storybook": "start-storybook",
  "storybook:build": "build-storybook"
},
```

## Usage

Once for all (and everytime you change anything into your `.storybook` config folder), you will have to run :

```bash
npm run storybook:build
```
in order to generate storybook's static assets.

Then, to run storybook, simply run :

```bash
npm run storybook
```

### CLI configuration

#### Dev server

The storybook server is based on [es-dev-server](https://open-wc.org/developing/es-dev-server.html) and accepts the 
same command line args defined in `.storybook/main.js`'s `esDevServer` exported property.  
Check the docs for all available options.

#### Storybook specific

| name             | type    | description                                                               |
| ---------------- | ------- | ------------------------------------------------------------------------- |
| config-dir        | string  | Where the storybook config files are. Default: `./.storybook`               |
| output-dir       | string  | Rollup build output directory. Default: `./static-storybook`              |
| absolute-imports | boolean | Allows to serve storybook files using absolute paths (disabled by default) |

### Configuration file

By default, storybook looks for a config file called `main.js` in your config dir (default `.storybook`). In this file you can configure storybook itself, `es-dev-server` and the `rollup` build configuration.

```js
module.exports = {
  // Globs of all the stories in your project
  stories: ['../stories/*.stories.{js,mdx}'],

  // Addons to be loaded, note that you need to import
  // them from storybook-prebuilt
  addons: [
    'storybook-prebuilt/addon-actions/register.js',
    'storybook-prebuilt/addon-knobs/register.js',
    'storybook-prebuilt/addon-a11y/register.js',
    'storybook-prebuilt/addon-docs/register.js',
  ],

  // Configuration for es-dev-server (start-storybook only)
  esDevServer: {
    nodeResolve: true,
    open: true,
  },

  // Rollup build output directory (build-storybook only)
  outputDir: '../dist',
  // Configuration for rollup (build-storybook only)
  rollup: config => {
    return config;
  },
};
```

### Create documentation (mdjs)

Create a `*.stories.md` (for example `card.stories.md`) file within the `stories` folder.

This uses the [Markdown JavaScript (mdjs) Format](https://open-wc.org/mdjs/) via [storybook-addon-markdown-docs](https://open-wc.org/demoing/storybook-addon-markdown-docs.html).

````md
```js script
import '../demo-wc-card.js';

export default {
  title: 'Demo Card/Docs (markdown)',
  parameters: { component: 'demo-wc-card' } },
};
```

# Demo Web Component Card

A component meant to display small information with additional data on the back.
// [...] use markdown to format your text
// the following demo is inline

```js story
export const Simple = () => html` <demo-wc-card>Hello World</demo-wc-card> `;
```

## Variations

Show demo with a frame and a "show code" button.

```js preview-story
export const Simple = () => html` <demo-wc-card>Hello World</demo-wc-card> `;
```

## API

The api table will show the data of "demo-wc-card" in your `custom-elements.json`.

<sb-props of="demo-wc-card"></sb-props>

// [...]
````

### Create documentation (mdx)

Create a `*.stories.mdx` (for example `card.stories.mdx`) file within the `stories` folder.

```md
import { Story, Preview, Meta, Props } from '@open-wc/demoing-storybook';
import { html } from 'lit-html';
import '../demo-wc-card.js';

<Meta title="Card|Docs" />

# Demo Web Component Card

A component meant to display small information with additional data on the back.
// [...] use markdown to format your text

<Preview withToolbar>
  <Story name="Simple" height="220px">
    {html`
      <demo-wc-card>Hello World</demo-wc-card>
    `}
  </Story>
</Preview>

## API

The api table will show the data of "demo-wc-card" in your `custom-elements.json`.

<Props of="demo-wc-card" />

// [...]
```

### Create stories in CSF (Component story format)

Create a `*.stories.js` (for example `card-variations.stories.js`) file within the `stories` folder.

```js
export default {
  title: 'Card|Variations',
  component: 'demo-wc-card',
};

export const singleComponent = () => html` <demo-wc-card></demo-wc-card> `;
```

For more details see the [official storybook docs](https://storybook.js.org/docs/formats/component-story-format/).

You can import these templates into any other place if needed.

For example in tests:

```js
import { expect, fixture } from '@open-wc/testing';
import { singleComponent } from '../stories/card-variations.stories.js';

it('has a header', async () => {
  const el = await fixture(singleComponent);
  expect(el.header).to.equal('Your Message');
});
```

### Create API playground

<div class="custom-block tip"><p class="custom-block-title">TIP</p> <p>You can find a more interactive version of this in the <a href="/demoing-storybook/?path=/docs/decorators-withwebcomponentknobs--example-output">withWebComponentsKnobs docs</a>.</p></div>

Base on the data in [custom-elements.json](./#custom-elementsjson) we can automatically generate knobs for your stories.

To enable this feature you will need to add an additional decorator.

**MDX**

```md
import { withKnobs, withWebComponentsKnobs } from '@open-wc/demoing-storybook';

<Meta
  title="WithWebComponentsKnobs|Docs"
  decorators={[withKnobs, withWebComponentsKnobs]}
  parameters={{ component: 'demo-wc-card', options: { selectedPanel: 'storybookjs/knobs/panel' } }}
/>

<Story name="Custom Header" height="220px">
  {html`
    <demo-wc-card header="Harry Potter">A character that is part of a book series...</demo-wc-card>
  `}
</Story>
```

**CSF**

```js
import { html } from 'lit-html';
import { withKnobs, withWebComponentsKnobs } from '@open-wc/demoing-storybook';

import '../demo-wc-card.js';

export default {
  title: 'Card|Playground',
  component: 'demo-wc-card',
  decorators: [withKnobs, withWebComponentsKnobs],
  parameters: { options: { selectedPanel: 'storybookjs/knobs/panel' } },
};

export const singleComponent = () => html` <demo-wc-card></demo-wc-card> `;
```

For additional features like

- define which components to show knobs for
- showing knobs for multiple different components
- syncing components states to knobs
- Filtering properties and debugging states

please see the official [documentation of the knobs for web components decorator](/demoing-storybook/?path=/docs/decorators-withwebcomponentknobs--example-output).

### custom-elements.json

In order to get documentation for web-components you will need to have a [custom-elements.json](https://github.com/webcomponents/custom-elementsjson) file.
You can hand write it or better generate it. Depending on the web components sugar you are choosing your mileage may vary.
Please not that the details of the file are still being discussed so we may adopt to changes in `custom-elements.json` without a breaking release.

Known analyzers that output `custom-elements.json`:

- [web-component-analyzer](https://github.com/runem/web-component-analyzer)
  - Supports LitElement, Polymer, Vanilla, (Stencil)
- [stenciljs](https://stenciljs.com/)
  - Supports Stencil (but does not have all metadata)

It basically looks like this:

```json
{
  "version": 2,
  "tags": [
    {
      "name": "demo-wc-card",
      "properties": [
        {
          "name": "header",
          "type": "String",
          "description": "Shown at the top of the card"
        }
      ],
      "events": [],
      "slots": [],
      "cssProperties": []
    }
  ]
}
```

For a full example see the [./demo/custom-elements.json](./demo/custom-elements.json).

### Additional middleware config like an api proxy

As we are using [es-dev-server](https://open-wc.org/developing/es-dev-server.html) under the hood you can use all it's power. You can use the regular command line flags, or provide your own config via `start storybook -c /path/to/config.js`.

To set up a proxy, you can set up a koa middleware. [Read more about koa here.](https://koajs.com/)

```javascript
const proxy = require('koa-proxies');

module.exports = {
  esDevServer: {
    port: 9000,
    middlewares: [
      proxy('/api', {
        target: 'http://localhost:9001',
      }),
    ],
  },
};
```
