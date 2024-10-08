# buildSrc-catalog

These two plugins aim to alleviate the long standing [issue](https://github.com/gradle/gradle/issues/15383) about having version catalogs accessible from precompiled script plugins

This is actually based on two plugins, a setting one and a project one

### buildSrc-catalog, the setting plugin

It shall be applied in your `buildSrc/settings.gradle.kts`

```
plugins {
    id("elect86.buildSrc-catalog")
}
```

It automatically includes any `libs.versions.toml` file found in the root `gradle` folder and generates, on configuration time, the accessors for all the elements found in the `lib` catalog: versions, libraries, bundles and plugins under the file `buildSrc/src/main/kotlin/libs.kt`. 

Generation will be skipped if not needed. It can detect and react to any `libs.versions.toml` modification

It also offers two confortable methods for adding a plugin as a dependency

Let's image you have the following:
  
`kotlin-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }`

in your `buildSrc/build.gradle.kts` you can simply write

```
import org.example.gradlePlugin

dependencies {
    implementation(libs.plugins.kotlin.serialization.gradlePlugin)
}
```

or also:

```
import org.example.implementation

dependencies {
    implementation(libs.plugins.kotlin.serialization)
}
```

### `plugins-catalog`, the project plugin


It shall be applied in your `buildSrc/build.gradle.kts`
```
plugins {
    id("elect86.plugins-catalog")
}
```

It allows to use accessors within the `plugins` block in precompiled plugins

the setting plugin above will generate the following accessor

```val PluginDependencySpecScope.`kotlin-serialization` ```
    
which can be use in your `buildSrc/src/main/kotlin/myPlugin.gradle.kts` as 

```
import `kotlin-serialization`

plugins {
    `kotlin-serialization`
}

```
