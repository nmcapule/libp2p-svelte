# libp2p-in-the-browser and in svelte

1. Start `js-libp2p-webrtc-star` server. Just run
   `npm run star-signal -- --port 3000 --host localhost`
2. Start me. Just run `npm run start:dev`

## HOW

Copy-paste most things from:
https://github.com/libp2p/js-libp2p/tree/master/examples/libp2p-in-the-browser

```
$ # needed coz some libp2p library imports a json file
$ npm install @rollup/plugin-json --save-dev
$ # go edit rollup.config.json to add the json() plugin
```

```
$ # needed coz some libp2p library uses 'buffer' import
$ npm install rollup-plugin-node-polyfills --save-dev
$ # go edit rollup.config.json to add the nodePolyfills plugin
```

```
$ # needed coz libp2p says so. damn.
$ npm install rollup-plugin-node-builtins rollup-plugin-node-globals --save-dev
$ # rollup-plugin-node-polyfills seems more updated but it blows up the compile :P
```
