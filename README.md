# Resource Generator
此为HBuilder Plus移植版，可生成符合HBuilder Plus要求的图标，ios上的启动图采用从cordova上移植过来的storyboard,也就是说需要自定义storyboard。

AndroidManifest.xml中的icon配置需修改为”mipmap/ic_launcher“。

This tool will crop and resize JPEG and PNG source images to generate icons and splash screens for modern iOS, Android, and Windows. `plus-res` was developed for use with Cordova, but Capacitor and other native runtimes are supported.

## Install

```bash
$ npm install -g @youtuosoft/plus-res
```

## Usage

`plus-res` expects a Cordova project structure such as:

```
resources/
├── icon.png
└── splash.png
config.xml
```

* `resources/icon.(png|jpg)` must be at least 1024×1024px
* `resources/splash.(png|jpg)` must be at least 2732×2732px
* `config.xml` is optional. If present, the generated images are registered accordingly

To generate resources with all the default options, just run:

```bash
$ plus-res
```

`plus-res` accepts a platform for the first argument. If specified, resources are generated only for that platform:

```bash
$ plus-res ios
```

Otherwise, if `config.xml` exists, `plus-res` will look for platforms (e.g. `<platform name="ios">`) and generate resources only for the configured platforms.

#### Documentation

See the help documentation on the command line with the `--help` flag.

```bash
$ plus-res --help
```

### Adaptive Icons

Android [Adaptive Icons](https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive) are also supported. If you choose to use them, create the following additional file(s):

* `resources/android/icon-foreground.png` must be at least 432×432px
* `resources/android/icon-background.png` must be at least 432×432px

A color may also be used for the icon background by specifying the `--icon-background-source` option with a hex color code, e.g. `--icon-background-source '#FFFFFF'`.

Regular Android icons will still be generated as a fallback for Android devices that do not support adaptive icons.

:memo: **Note**: For Cordova apps, Cordova 9+ and `cordova-android` 8+ is required.

### Capacitor

To use `plus-res` in Capacitor and other native runtimes, it is recommended to use `--skip-config` (skips reading & writing to Cordova's `config.xml` file) and `--copy` (copies generated resources into native projects).

For example, to generate icons and splash screens for iOS and Android in Capacitor, run:

```bash
$ plus-res ios --skip-config --copy
$ plus-res android --skip-config --copy
```

You can use `--ios-project` and `--android-project` to specify the native project directories into which these resources are copied. By default, `plus-res` copies Android resources into `android/` and iOS resources into `ios/` (the directories Capacitor uses).

### Tips

#### .gitignore

To avoid committing large generated images to your repository, you can add the
following lines to your `.gitignore`:

```
resources/android/icon
resources/android/splash
resources/ios/icon
resources/ios/splash
resources/windows/icon
resources/windows/splash
```

### Programmatic API

`plus-res` can be used programmatically.

#### CommonJS Example

```js
const run = require('plus-res');

await run();
```

#### TypeScript Example

`run()` takes an options object described by the interface `Options`. If options are provided, resources are generated in an explicit, opt-in manner. In the following example, only Android icons and iOS splash screens are generated.

```ts
import { Options, run } from 'plus-res';

const options: Options = {
  directory: '/path/to/project',
  resourcesDirectory: 'resources',
  logstream: process.stdout, // Any WritableStream
  platforms: {
    android: { icon: { sources: ['resources/icon.png'] } },
    ios: { splash: { sources: ['resources/splash.png'] } },
  },
};

await run(options);
```

### Cordova Reference Documentation

- plus sdk: https://nativesupport.dcloud.net.cn/AppDocs/README
