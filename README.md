# gradle-build-properties-plugin
[![](https://ci.novoda.com/buildStatus/icon?job=gradle-build-properties-plugin)](https://ci.novoda.com/job/gradle-build-properties-plugin/lastSuccessfulBuild/console) [![](https://img.shields.io/badge/License-Apache%202.0-lightgrey.svg)](LICENSE.txt) [![Bintray](https://api.bintray.com/packages/novoda/maven/gradle-build-properties-plugin/images/download.svg) ](https://bintray.com/novoda/maven/gradle-build-properties-plugin/_latestVersion)

External properties support for your gradle builds.

## Description

Gradle builds are highly configurable through various properties. Rather than hardcoding these
properties in your build scripts, for security reasons and in order to increase modularity, it's a
common practice to provide these properties from external sources. 

This plugin aims to provide a simple way to:
- consume properties from external and internal sources like cli, system properties, files etc.
- define a custom source for properties
- configure Android build with external properties 

## Adding to your project

The plugin is deployed to Bintray's JCenter. Ensure it's correctly defined
as a dependency for your build script:

```gradle
buildscript {
  repositories {
    jcenter()
  }
  dependencies {
    classpath 'com.novoda:gradle-build-properties-plugin:0.3'
  }
}
```
Then apply the plugin in your build script via:
```gradle
apply plugin: 'com.novoda.build-properties'
```

## Simple usage
Add a `buildProperties` configuration to your build script listing
all the properties files you intend to reference later on:
```gradle
buildProperties {
    
    secrets {
        using project.file('secrets.properties')
    }
}
```
where `secrets.properties` is a properties file that can now be referenced
in the build script as `buildProperties.secrets`. Entries in such file can be
accessed via the `getAt` operator:
```gradle
Entry entry = buildProperties.secrets['aProperty']
```

The value of an `Entry` can be retrieved via one of its typed accessors:

- `boolean enabled = buildProperties.secrets['x'].boolean`
- `int count = buildProperties.secrets['x'].int`
- `double rate = buildProperties.secrets['x'].double`
- `String label = buildProperties.secrets['x'].string`

It is important to note that values are lazy loaded too (via the internal closure provided in `Entry`).
Trying to access the value of a specific property could generate an exception if the key is missing in the provided properties file, eg:
```
FAILURE: Build failed with an exception.

* What went wrong:
A problem occurred configuring project ':app'.
> No value defined for property 'notThere' in 'secrets' properties (/Users/toto/novoda/spikes/BuildPropertiesPlugin/sample/properties/secrets.properties)

```

## Advanced usage

For more advanced configurations, please refer to the [advanced usage](docs/advanced-usage.md).
