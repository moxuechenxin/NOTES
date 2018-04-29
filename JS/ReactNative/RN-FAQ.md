#### 打包apk（执行./gradlew assembleRelease）：
```
node_modules_reactnavigation_src_views_assets_backicon.png: error: uncompiled PNG file passed as argument. Must be compiled first into .flat file..
```
解决：
```js
buildscript {
    // ...
    dependencies {
        // classpath 'com.android.tools.build:gradle:3.1.2'
        classpath 'com.android.tools.build:gradle:2.3.3'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

[参考](https://github.com/facebook/react-native/issues/16906)