<p>
  <img src="https://img.shields.io/badge/Swift-5.6-f05318.svg" />
  <img src="https://img.shields.io/badge/iOS->= 13.0-blue.svg" />
  <img src="https://img.shields.io/badge/macOS->= 10.15-blue.svg" />
  <img src="https://img.shields.io/badge/watchOS->= 6.0-blue.svg" />
  <img src="https://img.shields.io/badge/tvOS->= 13.0-blue.svg" />
  <img alt="GitHub" src="https://img.shields.io/github/license/lukepistrol/SwiftLintPlugin">
  <a href="https://twitter.com/lukeeep_">
    <img src="https://img.shields.io/badge/Twitter-@lukeeep_-1e9bf0.svg?style=flat" alt="Twitter: @lukeeep_" />
  </a>
</p>

# SwiftLintPlugin

A Swift Package Plugin for [SwiftLint](https://github.com/realm/SwiftLint/) that will run SwiftLint on build time and show errors & warnings in Xcode.

> **Note** 
> There now is an official version in the [SwiftLint repo](https://github.com/realm/SwiftLint#plug-in-support)!

## Add to Package

First add a dependency from this package:

```swift
dependencies: [
    // ...
    .package(url: "https://github.com/tzraikov/SwiftLintPlugin", from: "0.5.4"),
]
```

Then add it to your targets as a plugin:

```swift
targets: [
    .target(
        name: "YOUR_TARGET",
        dependencies: [],
        plugins: [
            .plugin(name: "SwiftLint", package: "SwiftLintPlugin")
        ]
    ),
]
```

## Add to Project

Starting with Xcode 14, plugins can also work on Xcode Project's targets. To do so, simply add this package to your SPM dependencies in Xcode. After that open your `target's settings > Build Phases` and add `SwiftLint` to `Run Build Tool Plug-ins` like shown below:

<img width="285" alt="Screen Shot 2022-09-02 at 09 33 23" src="https://user-images.githubusercontent.com/9460130/188084164-49903dc4-39a4-42fc-aa6f-6c6a813a7239.png">

> You may need to enable & trust the plugin before you can actually run it during builds.

## Run on CI

Important to notice is that when building a package/project on any CI provider (e.g. GitHub Actions) it is mandatory to pass the `-skipPackagePluginValidation` flag to the `xcodebuild` command. This will skip the validation prompt which in Xcode looks like this:

<img width="263" alt="Screenshot 2022-12-13 at 17 48 44" src="https://user-images.githubusercontent.com/9460130/207394170-9490e687-e066-4bfa-862c-a4f816b6b43b.png">

### Example

```bash
xcodebuild  \
    -scheme "$SCHEME" \
    -destination "$PLATFORM" \
    -skipPackagePluginValidation \ # this is mandatory
    clean build
```

If you need to disable linting (for release/app store builds), you can set`DISABLE_SWIFTLINT` environment variable
