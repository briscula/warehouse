# warehouse

Replenishing stock is one of the most important repetitive tasks performed by a retailer, yet for majority of the retail world this process is highly inefficient and time consuming. We've created a (first of its kind) open-source and free iPad application that makes the stock ordering process fast and fun, and frees up a whole lot of time for store managers and warehouse folks. To top it off, this app works beautifully with your inventory and POS! Just let us know which system(s) you use and we can add the integration. 

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/ShoppinPal/warehouse?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

### Development

Please follow the [Git-Flow Branching](http://blog.sourcetreeapp.com/2012/08/01/smart-branching-with-sourcetree-and-git-flow/) model when working with this repo.

In order to hack on this you'll need a few things installed and setup first:

#### Install These:
```
* [NodeJS] (http://nodejs.org)
  * __Recommended:__ Use [nvm](https://github.com/creationix/nvm) so you can have multiple NodeJS versions.
* [Ruby](https://www.ruby-lang.org/en/)
  * __Recommended:__ Use [rvm](http://rvm.io/) so you can have multiple versions of ruby and gemsets.
* [Yeoman](http://yeoman.io)
```

#### Clone it and then:
```
cd ~/dev/
git clone https://github.com/ShoppinPal/warehouse.git warehouse
cd warehouse
nvm install v0.10.37
nvm use v0.10.37
npm install -g yo
npm install
npm install -g bower
bower install
```

#### Setup a `server/config.development.json` file
```
{
  "restApiRoot": "/api",
  "host": "0.0.0.0",
  "port": 3000,
  "aclErrorStatus": 403,
  "remoting": {
    "json": {
      "limit": "50mb"
    }
  },
  "site": {
    "baseUrl": "http://localhost:3000",
    "proxyUrl": ""
  },
  "logging": {
    "console": true,
    "file": false
  },
  "prestashop":{
    "apiKey":""
  },
  "vend":{
    "auth_endpoint":"https://secure.vendhq.com/connect",
    "token_service":".vendhq.com/api/1.0/token",
    "client_id":"<fill it in>",
    "client_secret":"<fill it in>"
  }
}
```

#### Using Docker
1. build a docker image (replacing DOCKER_HANDLE and SUBDOMAIN is upto you)

  ```
  cd ~/dev/ && \
  git clone https://github.com/ShoppinPal/warehouse.git warehouse && \
  cd ~/dev/warehouse \
  docker build -t DOCKER_HANDLE/warehouse \
    --build-arg VM_EXTERNAL_IP=SUBDOMAIN.ngrok.io \
    --build-arg SEED_FILE=server/boot/seed.json \
    ~/dev/w2
  ```
1. run a container in detached state

  ```
  docker run \
     --env SKIP_SEEDING=false \
     --env MONGOLAB_URI=mongodb://localhost:27000/warehouse \
     --name warehouse \
     --publish 3000:3000 --detach DOCKER_HANDLE/warehouse
  ```
1. follow the logs

  ```
  docker logs --follow warehouse
  ```

#### For developing dependencies alongside:
```
cd ~/dev/
git clone https://github.com/ShoppinPal/vend-nodejs-sdk.git vend-nodejs-sdk
cd vend-nodejs-sdk
npm install
npm link
cd ~/dev/warehouse/
npm link vend-nodejs-sdk
```

#### Testing

* Unit tests: `grunt test`
* End-to-End Tests:
  * Setup:
    * `npm install --unsafe-perm`
    * `cd test/utils`
    * `npm install`
    * `cd ../..`
  * One Specific Test:
    * `grunt test:e2e --specs='test/protractor/e2eTest1.js'`
    * `grunt test:e2e --specs='test/protractor/e2eTest2.js'`
  * Multiple Tests
  * `grunt test:e2e --specs='test/protractor/e2eTest1.js,e2eTest2.js'`
  * All Tests
  * `grunt test:e2e`

#### Front-End

AngularJS is used as the client-side framework for this app and Bootstrap is used for the styling.

#### Back-End

The api is hosted by Loopback server (superset of Express) running on NodeJS.

### Workflow

This project is setup with Grunt tasks to make development and deployment easy.

#### Server

Launch your loopback server in development mode.
```
grunt server:development --subdomain <subdomain>
DEBUG=shoppinpal:*,boot:create-model-instances,boot:create-role-resolver grunt server:development --subdomain <subdomain>
```
 

