# Storybook Addon Next <!-- omit in toc -->

**😱 No config support for Next.js**: Tired of writing and debugging webpack config? What Next.js supports out of the box, this addon makes possible in Storybook

[![current version](https://img.shields.io/npm/v/storybook-addon-next.svg)](https://www.npmjs.com/package/storybook-addon-next)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)
[![semantic-release: angular](https://img.shields.io/badge/semantic--release-angular-e10079?logo=semantic-release)](https://github.com/semantic-release/semantic-release)

## Table of Contents <!-- omit in toc -->

- [Supported Features](#supported-features)
- [Requirements](#requirements)
- [Examples](#examples)
- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Register the Addon in main.js](#register-the-addon-in-mainjs)
  - [Partay](#partay)
- [Documentation](#documentation)
  - [Next.js's Image Component](#nextjss-image-component)
    - [Local Images](#local-images)
    - [Remote Images](#remote-images)
    - [Optimization](#optimization)
    - [AVIF](#avif)
  - [Next.js Routing](#nextjs-routing)
    - [Overriding defaults](#overriding-defaults)
    - [Global Defaults](#global-defaults)
    - [Default Router](#default-router)
    - [Actions Integration Caveats](#actions-integration-caveats)
  - [Sass/Scss](#sassscss)
  - [Css/Sass/Scss Modules](#csssassscss-modules)
  - [Styled JSX](#styled-jsx)
  - [Postcss](#postcss)
  - [Absolute Imports](#absolute-imports)
  - [Typescript](#typescript)
  - [next.config.js](#nextconfigjs)
  - [FAQ](#faq)
    - [Statically imported images won't load](#statically-imported-images-wont-load)
    - [This addon breaks when the .mjs extension for the next config is used](#this-addon-breaks-when-the-mjs-extension-for-the-next-config-is-used)
- [Similar Projects](#similar-projects)
- [Want to suggest additional features?](#want-to-suggest-additional-features)
- [Didn't find what you were looking for?](#didnt-find-what-you-were-looking-for)

## Supported Features

👉 [Next.js's Image Component](#nextjss-image-component)

👉 [Next.js Routing](#nextjs-routing)

👉 [Sass/Scss](#sassscss)

👉 [Css/Sass/Scss Modules](#csssassscss-modules)

👉 [Styled JSX](#styled-jsx)

👉 [Postcss](#postcss)

👉 [Absolute Imports](#absolute-imports)

👉 [Typescript](#typescript) (already supported out of the box by Storybook)

## Requirements

- [Next.js](https://nextjs.org/) >= 9.x
- [Storybook](https://storybook.js.org/) >= 6.x
  - Storybook webpack 5 builder
    - [Intro](https://storybook.js.org/blog/storybook-for-webpack-5/)
    - [Installation guide](https://gist.github.com/shilman/8856ea1786dcd247139b47b270912324)
    - It's not that this plugin can't support the webpack 4 builder, it's just that there hasn't been much of a need to and this is what Storybook recommends for nextjs apps. If you feel that you have a good use case, feel free to [open up an issue](https://github.com/RyanClementsHax/storybook-addon-next/issues).
- Your Next.js config file uses the `.js` extension and not the `.mjs` extension (i.e. `next.config.js` not `next.config.mjs`)
  - See [next.config.js](#nextconfigjs) for more details

## Examples

- Nextjs v12 - [Source](./examples/nextv12/README.md)
- Tailwindcss - [Source](./examples/with-tailwindcss/README.md)
- Nextjs v11.1 - [Source](./examples/nextv11_1/README.md)
- Nextjs v11.0 - [Source](./examples/nextv11_0/README.md)
- Nextjs v10 - [Source](./examples/nextv10/README.md)
- Nextjs v9 - [Source](./examples/nextv9/README.md)

## Getting Started

### Installation

Install `storybook-addon-next` using [`yarn`](https://yarnpkg.com/en/package/storybook-addon-next):

```bash
yarn add --dev storybook-addon-next
```

Or [`npm`](https://www.npmjs.com/package/storybook-addon-next):

```bash
npm install --save-dev storybook-addon-next
```

### Register the Addon in main.js

```js
module.exports = {
  addons: ['storybook-addon-next']
}
```

### Partay

🥳🎉 Thats it! The [supported features](#supported-features) should work out of the box.

See [Documentation](#documentation) for more details on how the supported features work in this addon.

If something doesn't work as you would expect, feel free to [open up an issue](https://github.com/RyanClementsHax/storybook-addon-next/issues).

## Documentation

### Next.js's Image Component

[next/image](https://nextjs.org/docs/api-reference/next/image) is [notoriously difficult](https://github.com/vercel/next.js/issues/18393) to get working with storybook. This addon allows you to use Next.js's `Image` component with no configuration!

#### Local Images

[Local images](https://nextjs.org/docs/basic-features/image-optimization#local-images) work just fine with this addon! Keep in mind that this feature was [only added in Next.js v11](https://nextjs.org/blog/next-11#automatic-size-detection-local-images).

```js
import Image from 'next/image'
import profilePic from '../public/me.png'

function Home() {
  return (
    <>
      <h1>My Homepage</h1>
      <Image
        src={profilePic}
        alt="Picture of the author"
        // width={500} automatically provided
        // height={500} automatically provided
        // blurDataURL="../public/me.png" set to equal the image itself (for this addon)
        // placeholder="blur" // Optional blur-up while loading
      />
      <p>Welcome to my homepage!</p>
    </>
  )
}
```

#### Remote Images

[Remote images](https://nextjs.org/docs/basic-features/image-optimization#remote-images) also work just fine!

```js
import Image from 'next/image'

export default function Home() {
  return (
    <>
      <h1>My Homepage</h1>
      <Image
        src="/me.png"
        alt="Picture of the author"
        width={500}
        height={500}
      />
      <p>Welcome to my homepage!</p>
    </>
  )
}
```

#### Optimization

All Next.js `Image`s are automatically [unoptimized](https://nextjs.org/docs/api-reference/next/image#unoptimized) for you.

If [placeholder="blur"](https://nextjs.org/docs/api-reference/next/image#placeholder) is used, the [blurDataURL](https://nextjs.org/docs/api-reference/next/image#blurdataurl) used is the [src](https://nextjs.org/docs/api-reference/next/image#src) of the image (thus effectively disabling the placeholder).

See [this issue](https://github.com/vercel/next.js/issues/18393) for more discussion on how Next.js `Image`s are handled for Storybook.

#### AVIF

This format is not supported by this addon yet. Feel free to [open up an issue](https://github.com/RyanClementsHax/storybook-addon-next/issues) if this is something you want to see.

### Next.js Routing

This solution is heavily based on [storybook-addon-next-router](https://github.com/lifeiscontent/storybook-addon-next-router) so a big thanks to `lifeiscontent` for providing a good solution that this addon could work off of.

[Next.js's router](https://nextjs.org/docs/routing/introduction) is automatically stubbed for you so that when the router is interacted with, all of its interactions are automatically logged to the [Storybook actions tab](https://storybook.js.org/docs/react/essentials/actions) if you have the actions addon.

#### Overriding defaults

Per-story overrides can be done by adding a `nextRouter` property onto the story [parameters](https://storybook.js.org/docs/react/writing-stories/parameters). The addon will shallowly merge whatever you put here into the router.

```js
import SomeComponentThatUsesTheRouter from "./SomeComponentThatUsesTheRouter";

export default {
  title: "My Story",
};

// if you have the actions addon
// you can click the links and see the route change events there
export const Example = () => <SomeComponentThatUsesTheRouter />;

Example.parameters: {
  nextRouter: {
    path: "/profile/[id]",
    asPath: "/profile/ryanclementshax",
    query: {
      id: "ryanclementshax"
    }
  }
}
```

See this [example](./examples/nextv12/stories/pages/nextjsRouting.stories.jsx) for a reference.

#### Global Defaults

Global defaults can be set in [preview.js](https://storybook.js.org/docs/react/configure/overview#configure-story-rendering) and will be shallowly merged with the default router.

```js
export const parameters = {
  nextRouter: {
    path: '/some-default-path',
    asPath: '/some-default-path',
    query: {}
  }
}
```

See this [example](./examples/nextv12/.storybook/preview.js) for a reference.

#### Default Router

The default values on the stubbed router are as follows (see [globals](https://storybook.js.org/docs/react/essentials/toolbars-and-globals#globals) for more details on how globals work)

```ts
const defaultRouter = {
  locale: context?.globals?.locale,
  route: '/',
  pathname: '/',
  query: {},
  asPath: '/',
  push(...args: unknown[]) {
    action('nextRouter.push')(...args)
    return Promise.resolve(true)
  },
  replace(...args: unknown[]) {
    action('nextRouter.replace')(...args)
    return Promise.resolve(true)
  },
  reload(...args: unknown[]) {
    action('nextRouter.reload')(...args)
  },
  back(...args: unknown[]) {
    action('nextRouter.back')(...args)
  },
  prefetch(...args: unknown[]) {
    action('nextRouter.prefetch')(...args)
    return Promise.resolve()
  },
  beforePopState(...args: unknown[]) {
    action('nextRouter.beforePopState')(...args)
  },
  events: {
    on(...args: unknown[]) {
      action('nextRouter.events.on')(...args)
    },
    off(...args: unknown[]) {
      action('nextRouter.events.off')(...args)
    },
    emit(...args: unknown[]) {
      action('nextRouter.events.emit')(...args)
    }
  },
  isFallback: false
}
```

#### Actions Integration Caveats

If you override a function, you lose the automatic action tab integration and have to build it out yourself.

```js
export const parameters = {
  nextRouter: {
    push() {
      // we lose the default implementation that logs the action into the action tab
    }
  }
}
```

Doing this yourself looks something like this (make sure you install the `@storybook/addon-actions` package):

```js
import { action } from '@storybook/addon-actions'

export const parameters = {
  nextRouter: {
    push(...args) {
      // custom logic can go here
      // this logs to the actions tab
      action('nextRouter.push')(...args)
      // return whatever you want here
      return Promise.resolve(true)
    }
  }
}
```

### Sass/Scss

[Global sass/scss stylesheets](https://nextjs.org/docs/basic-features/built-in-css-support#sass-support) are supported without any additional configuration as well. Just import them into [preview.js](https://storybook.js.org/docs/react/configure/overview#configure-story-rendering)

```js
import '../styles/globals.scss'
```

This will automatically include any of your [custom sass configurations](https://nextjs.org/docs/basic-features/built-in-css-support#customizing-sass-options) in your `next.config.js` file.

> Right now only the `.js` extension of the Next.js config is supported, not `.mjs`. See [next.config.js](#nextconfigjs) for more details.

```js
const path = require('path')

module.exports = {
  // any options here are included in sass compilation for your stories
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')]
  }
}
```

### Css/Sass/Scss Modules

Next.js supports [css modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css) out of the box so this addon supports it too.

```js
// this import works just fine in Storybook now
import styles from './Button.module.css'
// sass/scss is also supported
// import styles from './Button.module.scss'
// import styles from './Button.module.sass'

export function Button() {
  return (
    <button type="button" className={styles.error}>
      Destroy
    </button>
  )
}
```

### Styled JSX

The built in CSS in JS solution for Next.js is [styled-jsx](https://nextjs.org/docs/basic-features/built-in-css-support#css-in-js), and this addon supports that out of the box too, zero config.

```js
// This works just fine in Storybook with this addon
function HelloWorld() {
  return (
    <div>
      Hello world
      <p>scoped!</p>
      <style jsx>{`
        p {
          color: blue;
        }
        div {
          background: red;
        }
        @media (max-width: 600px) {
          div {
            background: blue;
          }
        }
      `}</style>
      <style global jsx>{`
        body {
          background: black;
        }
      `}</style>
    </div>
  )
}

export default HelloWorld
```

### Postcss

Next.js lets you [customize postcss config](https://nextjs.org/docs/advanced-features/customizing-postcss-config#default-behavior). Thus this addon will automatically handle your postcss config for you.

This allows for cool things like zero config tailwindcss! See the [with-tailwindcss example](examples/with-tailwindcss/README.md) for reference! Its a clone of [Next.js's tailwindcss example](https://github.com/vercel/next.js/tree/canary/examples/with-tailwindcss) set up with storybook and this addon.

### Absolute Imports

Goodbye `../`! Absolute imports from the root directory work just fine with this addon.

```js
// All good!
import Button from 'components/button'
// Also good!
import styles from 'styles/HomePage.module.css'

export default function HomePage() {
  return (
    <>
      <h1 className={styles.title}>Hello World</h1>
      <Button />
    </>
  )
}
```

```js
// preview.js

// Also ok in preview.js!
import 'styles/globals.scss'

// ...
```

### Typescript

There is no special thing this addon does to support [Typescript](https://www.typescriptlang.org/) because Storybook already supports it out of the box. I just listed it in the [supported features](#supported-features) for completeness and not to confuse anyone comparing the list of "out of the box" [features](https://nextjs.org/docs/getting-started) Next.js has with this addon.

### next.config.js

Right now the only supported config format for Next.js that this plugin supports is the commonjs version of the config (i.e. `next.config.js`). This is mostly because I haven't figured out how to require a `.mjs` file from a storybook addon (which is bound to commonjs modules as far as I know right now). If you are able to help, I'd love it if you could contribute to [this discussion](https://github.com/RyanClementsHax/storybook-addon-next/discussions/28) to get support for the `.mjs` version if such support is even possible.

### FAQ

#### Statically imported images won't load

Make sure you are treating image imports the same way you treat them when using next image in normal development.

Before `storybook-addon-next`, image imports just imported the raw path to the image (e.g. `'static/media/stories/assets/plugin.svg'`). When using `storybook-addon-next` image imports work the "Next.js way" meaning that we now get an object when we import an image. For example:

```json
{
  "src": "static/media/stories/assets/plugin.svg",
  "height": 48,
  "width": 48,
  "blurDataURL": "static/media/stories/assets/plugin.svg"
}
```

Therefore, if something in storybook isn't showing the image properly, make sure you expect the object to be returned from an import instead of just the asset path.

See [local images](https://nextjs.org/docs/basic-features/image-optimization#local-images) for more detail on how Next.js treats static image imports.

#### This addon breaks when the .mjs extension for the next config is used

Right now using `next.config.mjs` isn't supported by this addon. See [next.config.js](#nextconfigjs) for more details. Right now, it is required for you to use the `.js` extension instead. Feel free to help out on [this discussion](https://github.com/RyanClementsHax/storybook-addon-next/discussions/28) to get this supported.

## Similar Projects

- [storybook-addon-next-router](https://github.com/lifeiscontent/storybook-addon-next-router)

## Want to suggest additional features?

I'm open to discussion. Feel free to [open up an issue](https://github.com/RyanClementsHax/storybook-addon-next/issues).

## Didn't find what you were looking for?

Was this documentation insufficient for you?

Was it confusing?

Was it ... dare I say ... inaccurate?

If any of the above describes your feelings of this documentation. Feel free to [open up an issue](https://github.com/RyanClementsHax/storybook-addon-next/issues).
