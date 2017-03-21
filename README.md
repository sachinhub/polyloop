# \<polyloop\>

Loopback model schema based entry web component

## Install the Polymer-CLI

First, make sure you have the [Polymer CLI](https://www.npmjs.com/package/polymer-cli) installed. Then run `polymer serve` to serve your application locally.

## Viewing Your Application

```
$ polymer serve
```
## Prerequsites
These webcomponents require an API which can emit loopback model definition in following format - 
```
{
  "properties": {                       // Model properties as in the JSON file of loopback model.
    "code": {
      "id": true,
      "required": true,
      "type": "string",
      "comment": "checkpoint code"      // Comment is optional.
    },
    "name": {
      "type": "string",
      "comment": "name of the checkpoint"
    },
    "location": {
      "required": true,
      "type": "string",
      "comment": "checkpoint location - Station/Field"
    },
    "category": {
      "required": true,
      "type": "string",
      "comment": "Category of the checkpoint- one of IC, NP, NI, NC"
    },
    "handlingType": {
      "required": true,
      "type": "string",
      "comment": "handling type - delivery, pickup, mfnpickup"
    },
    "reasonCodes": {
      "type": [
        "string"
      ],
      "comment": "reasons on which this code can occur"
    }
  },
  "readonly": false,
  "name": "CheckpointMaster",                             // Name of the model.
  "strict": false,
  "public": true,
  "idInjection": false,
  "validateUpsert": false,
  "dataSourceName": "db",
  "idName": "code"                                        // Name of the ID field.
}
```

### Setting up ModelDefinition API in your loopback project
This project packages the required files to setup ModelDefinition API in your loopback project. Please follow these steps - 

#### 1 - Copy loopback/ModelDefinition.json to common/models
#### 2 - Add entry in model-config.json as below
```
"ModelDefinition": {
    "dataSource": "db",
    "public": true
  }
 ```
 
 #### 3 - Copy loopback/load-models-in-db.js into server/boot folder
 This boot script loads existings models into the database at the startup.
 
 #### 4 - Override ModelBuilder's define method to capture model properties 
Add below lines in server/server.js before app.start () call to override the ModelBuilder.define () method to capture model properties. This will be used by load-models-in-db.js script while storing the details in database.
```
const ModelBuilder = require('loopback-datasource-juggler').ModelBuilder;

const oldDefine = ModelBuilder.prototype.define;

ModelBuilder.prototype.define = function defineClass(className, properties, settings, parent) {
  const m = oldDefine.call(this, className, properties, settings, parent);
  m._ownProperties = properties;

  return m;
};
.
.
.
app.start = function () {

```
## Building Your Application

```
$ polymer build
```

This will create a `build/` folder with `bundled/` and `unbundled/` sub-folders
containing a bundled (Vulcanized) and unbundled builds, both run through HTML,
CSS, and JS optimizers.

You can serve the built versions by giving `polymer serve` a folder to serve
from:

```
$ polymer serve build/bundled
```

## Running Tests

```
$ polymer test
```

Your application is already set up to be tested via [web-component-tester](https://github.com/Polymer/web-component-tester). Run `polymer test` to run your application's test suite locally.
