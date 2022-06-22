# How to Deploy alveyinfo

alveyinfo is splitted into 3 repos:
* [https://github.com/alveycoin/alveyinfo](https://github.com/alveycoin/alveyinfo)
* [https://github.com/alveycoin/alveyinfo-api](https://github.com/alveycoin/alveyinfo-api)
* [https://github.com/alveycoin/alveyinfo-ui](https://github.com/alveycoin/alveyinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy alvey core
1. `git clone --recursive https://github.com/alveyproject/alvey.git --branch=alveyinfo`
2. Follow the instructions of [https://github.com/alveycoin/alvey/blob/master/README.md#building-alvey-core](https://github.com/alveycoin/alvey/blob/master/README.md#building-alvey-core) to build alvey
3. Run `alveyd` with `-logevents=1` enabled

## Deploy alveyinfo
1. `git clone https://github.com/alveyproject/alveyinfo.git`
2. `cd alveyinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `alveyinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `alveyinfo` under a process manager (like `pm2`), to restart the process when `alveyinfo` crashes.

## Deploy alveyinfo-api
1. `git clone https://github.com/alveycoin/alveyinfo-api.git`
2. `cd alveyinfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy alveyinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/alveycoin/alveyinfo-ui.git`
2. `cd alveyinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "ALVINFO_API_BASE_CLIENT=/api/ ALVINFO_API_BASE_SERVER=http://localhost:3001/ ALVINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `alveyinfo-ui` on port 3000
4. `npm run build`
5. `npm start`
