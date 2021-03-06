# MapStore Standard project

The standard project provides a customizable webgis application.
The command `npx @mapstore/project create standard` or `npx @mapstore/project create` generates a MapStore standard project with the structure represented below.

- [Project structure](#project-structure)
- [Configuration](#configuration)

## Project structure

```
standard-project/
|-- assets/ (optional)
|    |-- img/
|    |    |-- favicon.ico
|    |    +-- ... others static image files
|    +-- ... assets files similar to static
|-- backend/
|-- configs/
|    |-- localConfig.json
|    |-- new.json
|    |-- newgeostory.json
|    +-- ... others config files
|-- js/
|    |-- apps/
|    |    |-- mapstore.jsx
|    |    +-- ... others webpack entry
|    +-- ... others js files
|-- themes/ (optional)
|    |-- default/
|    |    |-- theme.less
|    |    +-- ...
|    +-- ... others themes
|-- translations/ (optional)
|    |-- data.en-US.json
|    +-- ... others translations
|-- .gitignore
|-- README.md
|-- index.ejs (optional)
|-- ... others html templates (ejs)
|-- devServer.js (optional)
|-- package.json
```

Folders description:

- `assets` this folder contains all the additional static resources
- `configs` this folder contains all the json configuration files
- `js` this folder contains all the custom javascript files of the project
  - `js/apps` each file in this folder becomes automatically a javascript entry. This folder must contain only javascript files
- `themes` each directory in this folder becomes automatically a theme entry. The name of the folder becomes the name of the style and a theme.less file is needed to represent the main theme entry
- `translations` custom translations for the project

## Configuration and customizations

### Custom configuration in package.json

The `mapstore` property inside the package.json allows to override and/or customize some configuration of the project. These are the available parameters:

- `templateParameters` _{object}_ overrides parameters of default html templates (index.ejs, embedded.ejs, api.ejs and unsupportedBrowser.ejs)
- templateParameters.`titleIndex` _{string}_
- templateParameters.`titleEmbedded` _{string}_
- templateParameters.`titleApi` _{string}_
- templateParameters.`favicon` _{string}_
- templateParameters.`loadingMessageIndex` _{string}_
- templateParameters.`loadingMessageEmbedded` _{string}_
- templateParameters.`titleUnsupported` _{string}_

Example of mapstore configuration in package.json:

```js
{
    // ...others package.json properties,
    "mapstore": {
        "templateParameters": {
            "favicon": "path/to/favicon"
        }
    }
}
```

### Override devServer.js

The devServer.js must return a function that returns a webpack devServer object.

Example of devServer.js:

```js
function devServer(devServerDefaultConfiguration) {
    return {
        ...devServerDefaultConfiguration,
        proxy: {
            ...devServerDefaultConfiguration.proxy,
            '/geoserver': {
                target: 'http://localhost:8080'
            }
        }
    };
}
module.exports = devServer;
```

