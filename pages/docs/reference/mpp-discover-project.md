---
type: doc
layout: reference
title: "了解项目"
---

# 了解项目

Discover main parts of your multiplatform project:

* [多平台插件](#多平台插件)
* [目标](#目标)
* [源集](#源集)
* [编译项](#编译项)

## 多平台插件

When you [create a multiplatform project](mpp-create-lib.html), the Project Wizard automatically applies the `kotlin-multiplatform` Gradle 
plugin in the file `build.gradle`(`.kts`).

You can also apply it manually.

>The `kotlin-multiplatform` plugin works with Gradle 6.0 or later. 
{:.note}

<div class="multi-language-sample" data-lang="groovy">
<div class="sample" markdown="1" theme="idea" mode="groovy">

```groovy
plugins {
    id 'org.jetbrains.kotlin.multiplatform' version '{{ site.data.releases.latest.version }}'
}
```

</div>
</div>

<div class="multi-language-sample" data-lang="kotlin">
<div class="sample" markdown="1" theme="idea" mode="kotlin" data-highlight-only>

```kotlin
plugins {
    kotlin("multiplatform") version "{{ site.data.releases.latest.version }}"
}
```

</div>
</div>

The `kotlin-multiplatform` plugin configures the project for creating an application or library to work on multiple platforms 
and prepares it for building on these platforms. 

In the file `build.gradle`(`.kts`), it creates the `kotlin` extension at the top level, which includes 
configuration for [targets](#targets), [source sets](#source-sets), and dependencies.


## 目标

A multiplatform project is aimed at multiple platforms that are represented by different targets. A target is part of the 
build that is responsible for building, testing, and packaging the application for a specific platform, such as macOS, 
iOS, or Android. See the list of [supported platforms](mpp-supported-platforms.html).

When you create a multiplatform project, targets are added to the `kotlin` block in the file `build.gradle` (`build.gradle.kts`).

<div class="sample" markdown="1" theme="idea" mode="kotlin" data-highlight-only>

```kotlin
kotlin {
    jvm()    
    js {
        browser {}
    }
 }
```

</div>

Learn how to [set up targets manually](mpp-set-up-targets.html).

## 源集

The project includes the directory `src` with Kotlin source sets, which are collections of Kotlin code files, along with 
their resources, dependencies, and language settings. A source set can be used in Kotlin compilations for one or more 
target platforms. 

Each source set directory includes Kotlin code files (the `kotlin` directory) and `resources`. The Project Wizard creates 
default source sets for the `main` and `test` compilations of the common code and all added targets. 

<img class="img-responsive" src="{{ url_for('asset', path='images/reference/mpp/source-sets.png' )}}" alt="Source sets" width="300"/>

>Source set names are case sensitive.
{:.note}

Source sets are added to the `sourceSets` block of the top-level `kotlin` block.

<div class="multi-language-sample" data-lang="groovy">
<div class="sample" markdown="1" theme="idea" mode="groovy">

```groovy
kotlin {
    sourceSets {
        commonMain { /* ... */} 
        commonTest { /* ... */}
        jvmMain { /* ... */}
        jvmTest { /* ... */ }
        jsMain { /* ... */}
        jsTest { /* ... */}    
    }
}
```

</div>
</div>

<div class="multi-language-sample" data-lang="kotlin">
<div class="sample" markdown="1" theme="idea" mode="kotlin" data-highlight-only>


```kotlin
kotlin {
    sourceSets {
        val commonMain by getting { /* ... */ }
        val commonTest by getting { /* ... */ }
        val jvmMain by getting { /* ... */ }
        val jvmTest by getting { /* ... */ } 
        val jsMain by getting { /* ... */ }
        val jsTest by getting { /* ... */ } 
    }
}
```

</div>
</div>

Source sets form a hierarchy, which is used for sharing the common code. In a source set shared among several targets, 
you can use the platform-specific language features and dependencies that are available for all these targets.

For example, all Kotlin/Native features are available in the `desktopMain` source set, which targets the Linux (`linuxX64`), 
Windows (`mingwX64`), and macOS (`macosX64`) platforms.

![Hierarchical structure]({{ url_for('asset', path='images/reference/mpp/hierarchical-structure.png') }})

Learn how to [build the hierarchy of source sets](mpp-share-on-platforms.html#share-code-on-similar-platforms). 

## 编译项

Each target can have one or more compilations, for example, for production and test purposes.

For each target, default compilations include:

*   `main` and `test` compilations for JVM, JS, and Native targets.
*   A compilation per [Android build variant](https://developer.android.com/studio/build/build-variants), for Android targets.

![Compilations]({{ url_for('asset', path='images/reference/mpp/compilations.png') }})

Each compilation has a default source set, which contains sources and dependencies specific to that compilation.

Learn how to [configure compilations](mpp-configure-compilations.html). 
