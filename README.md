# example-confippet-hapijs
* TBD

## Instructions
* You can build the app from scratch by following the instructions below:
  * [Hapijs Server](#hapijs-server)
  * [Electrode Confippet](#electrode-confippet)

## <a name="hapijs-server"></a>Hapijs Server

### Install
* Create a hapi app using the following commands:

```
mkdir hapiApp
cd hapiApp
npm init
npm install hapi --save
npm install inert --save
```

### Server
* Create a `server.js` file using this code:

```js
'use strict';

const Hapi = require('hapi');
const Path = require('path');
const Inert = require('inert');
const server = new Hapi.Server({
  connections: {
    routes: {
      files: {
        relativeTo: Path.join(__dirname, 'public')
      }
    }
  }
});
const config = {
  connection: {
    port: 3000
  }
};

server.connection(config.connection);
server.register(Inert, () => {});
server.route({
  method: 'GET',
  path: '/{param*}',
  handler: {
    directory: {
      path: '.',
      redirectToSlash: true,
      index: true
    }
  }
});
server.route({
  method: 'GET',
  path: '/',
  handler: function (request, reply) {
    reply('Hapijs Server Running...');
  }
});

server.start((error) => {
  if (error) {
    throw error;
  }
  console.log(`hapijs server running @ ${server.info.uri}`);
});
```

## <a name="electrode-confippet"></a>Electrode Confippet
* [Confippet](https://github.com/electrode-io/electrode-confippet) is a versatile utility for managing your NodeJS
application configuration. Its goal is customization and extensibility, but offers a preset config out of the box.

### Install

```bash
npm install electrode-confippet --save
```

### Configure
* Create the config folder:

```
mkdir config
cd config
```

* Add the following config files:

```
config
|_ default.json
|_ development.json
|_ production.json
```

* Add your configuration settings

#### Default
* Update the `config/default.json` to have the following settings:

```
{
  "connection": {
    "port": 3000
  }
}
```

#### Development environment
* Update the `config/development.json` to have the following settings:

```
{
  "connection": {
    "port": 4000
  }
}
```

* The above settings run the server in port 4000

#### Production environment
- Update the `config/production.json` to have the following settings:

```
{
  "connection": {
    "port": 8000
  }
}
```

* The above settings run the server in port 8000
* Keys that exist in the `config/default.json` that are also in the other environment configs will be replaced by the environment specific versions

### Usage
* Replace the config line with the following in `server.js`:

```
const config = require("electrode-confippet").config;
```

### Run
* Start the hapijs app in `development` environment:

```
NODE_ENV=development npm start
```

* Start the hapijs app in `production` environment:

```
NODE_ENV=production npm start
```

* Running in the selected environment should load the appropriate configuration settings
