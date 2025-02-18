# A Chrome extension for eco-conscious shopping

Built using the svelte chrome extension [boilerplate](https://github.com/d-lowl/chrome-extension-svelte-boilerplate). Data stored in an [airtable](https://airtable.com/invite/l?inviteId=invIyZVcyifUauhPX&inviteToken=1cd27e37721480eec5017a7179c044a61581384240d82720f17cf5cd02fa5542) (for now). Plans to integrate this plugin with the larger [open climate lab ecosystem](https://github.com/YaleOpenLab/Collabathon_2019). Initially developed during the first Yale OpenClimate Lab [Collabathon](https://www.collabathon.openclimate.earth/). 
It draws inspiration from the [Honey](https://www.joinhoney.com/) amazon shopping extension. The refined github extension [source](https://github.com/sindresorhus/refined-github)

## Developing on the extension
Get up to speed with the slick frontend framework with [Svelte docs](https://svelte.technology/guide),  and the [Chrome Extension](https://developer.chrome.com/extensions/getstarted) reference will be useful. 

## Contributing
A great way to get started is to check for good first issues on the tracker. Overall we try to plan and execute using the Kanban boards in github [projects](https://github.com/starling-foundries/rhizomatic/projects).
We use gitflow and welcome pull requests from new collaborators of all backgrounds and affiliations.



## Getting started 

1. Check if your Node.js version is >= 6.
2. Clone the repository.
3. Install [yarn](https://yarnpkg.com/lang/en/docs/install/).
4. Run `yarn`.
5. Change the package's name and description on `package.json`.
6. Change the name of your extension on `src/manifest.json`.
7. Run `npm run start`
8. Load your extension on Chrome following:
    1. Access `chrome://extensions/`
    2. Check `Developer mode`
    3. Click on `Load unpacked extension`
    4. Select the `build` folder.
8. Have fun.

## Structure
All your extension's development code must be placed in `src` folder, including the extension manifest.

The boilerplate is already prepared to have a popup, a options page and a background page. You can easily customize this.

Each page has its own assets package defined. So, to code on popup you must start your code on `src/js/popup.js`, for example.

Svelte components are under `src/svelte/*.svelte`. The example popup component is provided at `src/svelte/popup.svelte`. .svelte extension is used to distinguish components from static html files, required for an extension.

## Webpack auto-reload and HRM
To make your workflow much more efficient this boilerplate uses the [webpack server](https://webpack.github.io/docs/webpack-dev-server.html) to development (started with `npm start`) with auto reload feature that reloads the browser automatically every time that you save some file o your editor.

You can run the dev mode on other port if you want. Just specify the env var `port` like this:

```
$ PORT=6002 npm start
```

## Content Scripts

Although this boilerplate uses the webpack dev server, it's also prepared to write all your bundles files on the disk at every code change, so you can point, on your extension manifest, to your bundles that you want to use as [content scripts](https://developer.chrome.com/extensions/content_scripts), but you need to exclude these entry points from hot reloading [(why?)](https://github.com/samuelsimoes/chrome-extension-webpack-boilerplate/issues/4#issuecomment-261788690). To do so you need to expose which entry points are content scripts on the `webpack.config.js` using the `chromeExtensionBoilerplate -> notHotReload` config. Look the example below.

Let's say that you want use the `myContentScript` entry point as content script, so on your `webpack.config.js` you will configure the entry point and exclude it from hot reloading, like this:

```js
{
  …
  entry: {
    myContentScript: "./src/js/myContentScript.js"
  },
  chromeExtensionBoilerplate: {
    notHotReload: ["myContentScript"]
  }
  …
}
```

and on your `src/manifest.json`:

```json
{
  "content_scripts": [
    {
      "matches": ["https://www.google.com/*"],
      "js": ["myContentScript.bundle.js"]
    }
  ]
}

```

## Packing
After the development of your extension run the command

```
$ NODE_ENV=production npm run build
```
Now, the content of `build` folder will be the extension ready to be submitted to the Chrome Web Store. Just take a look at the [official guide](https://developer.chrome.com/webstore/publish) to more infos about publishing.

## Secrets
If you are developing an extension that talks with some API you probably are using different keys for testing and production. Is a good practice you not commit your secret keys and expose to anyone that have access to the repository.

To this task this boilerplate import the file `./secrets.<THE-NODE_ENV>.js` on your modules through the module named as `secrets`, so you can do things like this:

_./secrets.development.js_

```js
export default { key: "123" };
```

_./src/popup.js_

```js
import secrets from "secrets";
ApiCall({ key: secrets.key });
```
:point_right: The files with name `secrets.*.js` already are ignored on the repository.

## Acknowledgement
I would like to thank Samuel Simões for [Chrome Extension Webpack Boilerplate](https://github.com/samuelsimoes/chrome-extension-webpack-boilerplate), which was used as a base for this project. Some of the readme instructions were also reused. 

