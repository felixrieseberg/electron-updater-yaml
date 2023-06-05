# electron-updater-yaml [![npm][npm_img]][npm_url]

A zero-dependencies module that generates the `yml` files required by `electron-updater`. Supports
both Windows and macOS. 

It is most useful if you're trying to migrate away from `electron-builder` but still want to use
`electron-updater`.

# Usage

This module is best integrated into your build pipeline. It exports two methods:

 - `getAppUpdateYml(options)`: Returns the contents of an `app-update.yml`. You should write this file into your app packages `resources` folder.
 - `getChannelYml(options)`: Returns the contents of an `latest.yml` file. The filename is depending on your
   update channel. This file should only be generated after you've created your zip/dmg/exe installers so that
   the generated sha512 hashes are accurate. You should upload this file to your update server.

```ts
import { getAppUpdateYml } from "electron-updater-yaml"

interface AppUpdateYmlOptions {
  // Name of your application.
  name: string;
  // URL to the location of your yml files. If your channel file (say, latest.yml) currently
  // lives at https://s3-us-west-2.amazonaws.com/my-bucket/latest.yml, you should set this
  // property to "https://s3-us-west-2.amazonaws.com/my-bucket"
  url: string;
  // Name of your channel. Defaults to "latest".
  channel?: string;
  // Name of your updater cache directory name. Defaults to "${name}-latest".
  updaterCacheDirName?: string;
}

const appUpdateYml = await getAppUpdateYml(options);
await fs.writeFile("path/to/resources/app-update.yml", appUpdateYml, "utf8")
```

```ts
import { getChannelYml } from "electron-updater-yaml"

interface ChannelYmlOptions {
  // Path to the location of your installers - typically an "out" or "dist" folder.
  // This tool will be scanning this folder for installers to include in the channel.yml, so
  // it should _only_ contain your installers. On Windows, we'll look for .exe files. On
  // macOS, we'll look for .zip and .dmg files.
  installerPath: string;
  // Version of your installer. This should match the version of your application.
  version: string;
  // Release date of your installer. Defaults to the current date.
  releaseDate?: string;
  // Platform to use. Defaults to the host platform (process.platform).
  platform?: Platform;
}

const channelYml = await getChannelYml(options);
await fs.writeFile("path/to/out/latest.yml", channelYml, "utf8")
```

# License
MIT. Please see LICENSE for details.

[electron]: https://github.com/electron/electron
[npm_img]: https://img.shields.io/npm/v/electron-updater-yaml.svg
[npm_url]: https://npmjs.org/package/electron-updater-yaml
