> Get free disk space info from [`df -kP`](https://en.wikipedia.org/wiki/Df_\(Unix\))

Works on any Unix-based system like macOS and Linux.

*Created because all the other `df` wrappers are terrible. This one uses simple and explicit parsing. Uses `execFile` rather than `exec`. Ensures better platform portability by using the `-P` flag. Returns sizes in bytes instead of kilobytes and the capacity as a float.*

**Forked from** [sindresorhus/df](//github.com/sindresorhus/df)

## Install

```
$ npm install @mendelgusmao/df
```


## Usage

```js
const df = require('@mendelgusmao/df');

(async () => {
	console.log(await df());
	/*
	[
		{
			filesystem: '/dev/disk1',
			type: 'ext4',
			size: 499046809600,
			used: 443222245376,
			available: 55562420224,
			capacity: 0.89,
			mountpoint: '/'
		},
		…
	]
	*/

	console.log(await df.fs('/dev/disk1'));
	/*
	{
		filesystem: '/dev/disk1',
		…
	}
	*/

	console.log(await df.file(__dirname));
	/*
	{
		filesystem: '/dev/disk1',
		…
	}
	*/
})();
```


## API

### df(args)

Returns a `Promise<Object[]>` with a list of space info objects for each filesystem.

### df.fs(path, args)

Returns a `Promise<Object>` with the space info for the given filesystem path.

- `filesystem` - Name of the filesystem.
- `type` - Type of the filesystem.
- `size` - Total size in bytes.
- `used` - Used size in bytes.
- `available` - Available size in bytes.
- `capacity` - Capacity as a float from `0` to `1`.
- `mountpoint` - Disk mount location.

#### path

Type: `string`

Path to a [filesystem device file](https://en.wikipedia.org/wiki/Device_file). Example: `'/dev/disk1'`.

### df.file(path, args)

Returns a `Promise<Object>` with the space info for the filesystem the given file is part of.

#### path

Type: `string`

Path to a file on the filesystem to get the space info for.

#### args

Type: `string[]`

For all methods, args is an array of arguments to be passed to `df`
