# demofile

[![Join the chat at https://gitter.im/saul/demofile](https://badges.gitter.im/saul/demofile.svg)](https://gitter.im/saul/demofile?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A node.js library for parsing modern (protobuf) Source 1 demo files.

## Supported Games

- Counter-Strike: Global Offensive

## Supported Demo Features

- [x] Entity updates, server classes, data tables (including instance baselines)
- [x] Both perspectives (GOTV and client-side recorded)
- [x] User messages
- [x] String tables
- [x] Game events
- [ ] User commands
- [ ] Console commands

## Installation

    npm install --save demofile

Generate documentation to `./docs/`:

    npm run generate-docs

## Example

Print all player information to console:

```js
var demofile = require('demofile');
var fs = require('fs');

fs.readFile('test.dem', function (err, buffer) {
  var demo = new demofile.DemoFile();

  demo.stringTables.on('update', e => {
    if (e.table.name === 'userinfo' && e.userData != null) {
      console.log('Player info updated:');
      console.log(e.entryIndex, e.userData);
    }
  });

  demo.parse(buffer);
});
```

## Release History

### 0.2.11 (16/10/16)

- :sparkles: Added entities `isHandleSet` to determine if a networked entity handle is set (the game uses `(1 << 21) - 1` to mean empty )

### 0.2.10 (11/10/16)

- :sparkles: Added entities `getByHandle` to find an entity from a given networked entity handle (e.g., from m_hMyWeapons)

### 0.2.9 (12/09/16)

- :sparkles: Added entities `baselineupdate` event which is fired whenever the `instancebaseline` string table is updated with new properties.

### 0.2.8 (30/08/16)

- :sparkles: The `dem_StringTables` command, which holds additional string table data on client-side recorded demos, is now parsed
- :zap: String table `postcreate` is now fired after `svc_CreateStringTable` and `dem_StringTables` are parsed

### 0.2.7 (23/08/16)

- :sparkles: Added entities `datatablesready` event which is fired when data tables have been parsed.

### 0.2.6 (23/08/16)

- :sparkles: Added entity `postcreate` event which is fired after an entity has been created and all of its properties parsed.

### 0.2.5 (08/07/16)

- :sparkles: Added string table `postcreate` event which is fired after a new stringtable's entries have been populated

### 0.2.4 (08/07/16)

- :bug: Fixed retrieving properties on entities whose server class has no instance baseline

### 0.2.3 (03/07/16)

- :sparkles: Instance baselines are now parsed and used by `Entity#getProp`

### 0.2.2 (03/07/16)

- :bug: Fixed stringtable updates affecting all entries in the table

### 0.2.1 (25/03/16)

- :card_index: Removed `node-protobuf` un-used dependency

### 0.2.0 (30/01/16)

- :zap: Each tick is now parsed on a separate process tick.
- :zap: Game event callbacks are now fired at the end of the tick.
- :bug: Fixed Vector props decoding to `undefined`.

### 0.1.0 (18/01/16)

- :tada: Initial release.

## Acknowledgements

- [CS:GO Protobuf sources copied from SteamKit](https://github.com/SteamRE/SteamKit)
