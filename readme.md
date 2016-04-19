# windows-fs

[![NPM version][version-image]][version-url]
[![Dependency Status][david-image]][david-url]
[![License][license-image]][license-url]
[![Js Standard Style][standard-image]][standard-url]

Windows utilities when working with the file system. Intended for use with [electron](http://electron.atom.io/) or nodejs.

## Installation

```bash
npm install windows-fs
```

## Example

```js
import { mount, getStats } from 'windows-fs'

mount('server', 'folder')
  .then(letter => {
    return getStats(letter)
  })
```

## API

### getDirSize

Gets the directory size (in bytes) using a recursive walk.

**Parameters**

-   `path` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Absolute path

**Examples**

```javascript
getDirSize('c:/temp/log')
// -> 32636
```

Returns **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** Directory size in bytes

### getStats

Gets stats by a given drive `letter` like the current size of the hdd etc.
This also works for drives in a network.

**Parameters**

-   `letter` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The drive letter which the hdd was mounted on

**Examples**

```javascript
getStats('Z:')
// -> { freeSpace: 10700152832, size: 53579083776 }
```

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)** A promise which resolves to `{ freeSpace, size }`

### mount

Mounts a network drive to the next available drive Letter and returns a
tuple the drive letter which it was mounted on.

**Parameters**

-   `unc` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** A UNC path like `//server`
-   `path` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** A path like `some/path/to/folder`
-   `credentials` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)=** `user` and `password` to log into a network

**Examples**

```javascript
// (given letter Y: is free)
mount('server', 'c$')
  .then((letter) => console.log(letter))
  .catch((err) => console.error('failed to mount', err))
// -> Y:
```

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)** Resolves to the drive letter which the path was
mounted on, rejects when the command fails

### toUncPath

Creates a windows path given a `server` name and a unix `path`.

**Parameters**

-   `server` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Server name
-   `share` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Path

**Examples**

```javascript
toUncPath('server', 'some/path/to/a/log')
// -> `\\server\some\path\to\a\log`
```

### toWindowsPath

Replaces `/` with `\` so we can use an unix path and convert it to a
windows path.

**Parameters**

-   `path` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Unix style path

**Examples**

```javascript
toWindowsPath('some/random/folder')
// -> some\random\folder
```

Returns **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Windows style path

### unmount

Unmounts a network drive given a `letter` and returns the `letter`.

**Parameters**

-   `letter` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Network drive letter

**Examples**

```javascript
// (given letter Z: is mounted)
unmount('Z:')
  .then((letter) => console.log(letter))
  .catch((err) => console.error('failed to unmount', err))
// -> Z:
```

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)** Resolves to the drive letter when successful, rejects
when the command fails

## Tests

```bash
npm test
```

## License

[MIT][license-url]

[version-image]: https://img.shields.io/npm/v/windows-fs.svg?style=flat-square

[version-url]: https://npmjs.org/package/windows-fs

[david-image]: http://img.shields.io/david/kanton-aargau/windows-fs.svg?style=flat-square

[david-url]: https://david-dm.org/kanton-aargau/windows-fs

[standard-image]: https://img.shields.io/badge/code-standard-brightgreen.svg?style=flat-square

[standard-url]: https://github.com/feross/standard

[license-image]: http://img.shields.io/npm/l/windows-fs.svg?style=flat-square

[license-url]: ./license
