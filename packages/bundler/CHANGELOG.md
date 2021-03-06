# Change Log

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

<!-- ## Unreleased -->
<!-- Add new, unreleased changes here. -->

## 4.0.2 - 2018-06-28
* Fix NPM audit warnings.

## 4.0.1 - 2018-05-11
* Added missing `babel-types` to dependencies instead of relying on it transitively.

## 4.0.0 - 2018-05-08
* Fixed inline module scripts which used bare name specifiers to correctly resolve them using the module resolutions of the Analyzer.

## 4.0.0-pre.7 - 2018-05-03
* Dropped support for node v6. This is a soft break, as we aren't
  making any changes that are known to break node v6, but we're no longer testing against it. See our [node version support policy](https://www.polymer-project.org/2.0/docs/tools/node-support)
  for details.
* Added support for `import.meta.url` preservation in bundled modules.

## 4.0.0-pre.6 - 2018-04-25
- Bundled documents now have a `language` field. It will be `html` when the ast
  node is an HTML Parse5 node, and `js` when the ast node is a JS babel node.
- Rewritten dynamic `import()` calls now test if a bundle has exports before
  attempting to destructure bundle.

## 4.0.0-pre.5 - 2018-04-17
- Update for breaking Analyzer change.

## 4.0.0-pre.4 - 2018-04-02
- Fix issue where external script tags referencing bundled ES modules were not updated.

## 4.0.0-pre.3 - 2018-03-28
- [BREAKING] The `--in-html` and `--out-html` arguments for `bin/polymer-bundler` are now `--in-file` and `--out-file` because input filetypes are no longer limited to HTML.
- ES6 Module Bundling! Bundler can simultaneously process HTML imports and ES6 module imports, including inline `<script type="module">` scripts.
- Fixed issue where `export * from 'x'` did not export identifiers from module `x`.

## 4.0.0-pre.2 - 2018-03-20
- Upgraded to use `polymer-analyzer` version `3.0.0-pre.17`.
- Switched to `FsUrlResolver` from `PackageUrlResolver` as the default `UrlResolver` for bundling contexts.  This is because `PackageUrlResolver` is intended for component analysis, not application analysis.

## 4.0.0-pre.1 - 2018-03-07
- [BREAKING] Upgraded to use `polymer-analyzer` version `3.0.0-pre.13` which requires the `ResolvedUrl` type in nearly all places where URLs are exchanged and provides strict typing on URLs used globally.  Eliminated the local `UrlString` type.
- [BREAKING] Removed `relativeUrl` function from `url-utils`, shifting responsibility to `Bundler#analyzer.urlResolver.relative()`.
- [BREAKING] Removed `import-utils.ts` and migrated all HTML-specific bundling code out of it and out of `bundler.ts` into a new class `HtmlBundler`.  Renamed `ast-utils.ts` to `parse5-utils.ts` since "ast" is ambiguous when we have JavaScript ASTs coming in.
- [BREAKING] `Bundle` class now has a required `type` property as first argument to its constructor, with current possible values of `html-fragment` and `es6-module`.
- [BREAKING] The `DepsIndex` type returned from `buildDepsIndex` has changed to a single map of entrypoints to dependencies.
- Added support for `<script type="module">` and bundling of ES6 modules by following `import` statements.  Dynamic `import()` statements are treated similarly to the `<link rel="lazy-import">` tag in that the import target implicitly defines a new entrypoint/bundle.
- The `BundleResult` returned by `Bundler#bundle()` includes serialized contents instead of just the root AST node of bundled document.

## 3.1.1 - 2017-10-20
- Fixed an issue with moving deprecated CSS import links into templates reported in [Polymer CLI Issue #917](https://github.com/polymer/polymer-cli/issues/917).

## 3.1.0 - 2017-10-02
- Fixed [issue #596](https://github.com/Polymer/polymer-bundler/issues/596) where html import to bundle file itself was being injected due to bad path check condition in the inject step.
- Fixed [issue #600](https://github.com/Polymer/polymer-bundler/issues/600) where `<link rel=stylesheet>` inside a `<template>` was not inlined.  If you want to exclude a specific stylesheet from inlining, you can add its path the the `excludes` option (or `--exclude` on command line).

## 3.0.1 - 2017-07-17
- Fixed [issue #592](https://github.com/Polymer/polymer-bundler/issues/592) where the `--manifest-out` option of `bin/polymer-bundler` did not correctly include the basis document of shell files in inlined content list.

## 3.0.0 - 2017-07-14
- BREAKING: The following changes support polymer 2.x, but will break some Polymer 1.x projects which may rely on the rewriting of relative urls within style tags.  For those projects, set the `rewriteUrlsInTemplates` option to `true` or use `--rewriteUrlsInTemplates` at command-line.
  - Fixed [issue #579](https://github.com/Polymer/polymer-bundler/issues/579) where `url()` values inside `<style>` tags inside of `<dom-module>` tags of inlined html imports were rewritten without consideration of the module's `assetpath` property.
  - Fixed issue when stylesheet imports are inlined inside of a `<dom-module>` the url resolution now takes into consideration the `assetpath`.

## 2.3.1 - 2017-07-13
- Rolling back these changes from 2.3.0 release which break polymer 1.x bundling case and going to release them in polymer-bundler 3.x because they are breaking changes:
  - Fixed [issue #579](https://github.com/Polymer/polymer-bundler/issues/579) where `url()` values inside `<style>` tags inside of `<dom-module>` tags of inlined html imports were rewritten without consideration of the module's `assetpath` property.
  - Fixed issue when stylesheet imports are inlined inside of a `<dom-module>` the url resolution now takes into consideration the `assetpath`.

## 2.3.0 - 2017-07-12
- Fixed issue where inlined `<link rel="import" type="css" ...>` "stylesheet imports" (deprecated but still supported) were inlined in reverse order.  Fixes [issue #575](https://github.com/Polymer/polymer-bundler/issues/575).
- Added a `missingImports` property to the `Bundle` class (part of the `BundleManifest`).
- The JSON generated by `--manifest-out` now includes a `_missing` url collection when any are encountered that could not be loaded.
- Added missing `--rewrite-urls-in-templates` option to `bin/polymer-bundler`.
- Fixed issue where HTML imports which were not inlined, because they matched an entry in the `excludes` option, were being erroneously added to the bundle's `missingImports` set.
- Fixed [issue #579](https://github.com/Polymer/polymer-bundler/issues/579) where `url()` values inside `<style>` tags inside of `<dom-module>` tags of inlined html imports were rewritten without consideration of the module's `assetpath` property.
- Fixed issue when stylesheet imports are inlined inside of a `<dom-module>` the url resolution now takes into consideration the `assetpath`.

## 2.2.0 - 2017-06-28
- Fixed issue where bundles produced by shell strategy incorrectly included eager html imports of the shell. https://github.com/Polymer/polymer-bundler/issues/471
- Fixed the `--inline-scripts` and `--inline-css` options, previously inlining happened regardless of whether the options were actually set.  Important, if you use the `bin/polymer-bundler` CLI you must provide these options to inline scripts and css now.
- Fixed issue producing assetpath in application root. https://github.com/Polymer/polymer-bundler/issues/562
- Fixed issue where absolute paths were not handled properly. https://github.com/Polymer/polymer-bundler/issues/559
- Fixed issue where out-dir was essentially ignored for a single bundle case. https://github.com/Polymer/polymer-bundler/issues/560
- Fixed issue where, if a different version of polymer-analyzer was given to Bundler's constructor, it caused instanceof check failures and resulting in documents not being identified as documents; added more definitive error messaging for this scenario as stop-gap until we switch to a no instanceof version of polymer-analyzer interface.

## 2.1.0 - 2017-06-14
- Added a `--redirect` option to `bin/polymer-bundler`.
- Added a `--root` option to `bin/polymer-bundler`.
- Added support for preserving important comments of the form `<!--! ... -->` when using `stripComments`.
- The `excludes` option now supports excluding specific script and css file and containing folder urls.
- The `stripComments` behavior now removes comments inside templates.
- Fixed issue with incorrect URL rewrites in inlined CSS (when URL points outside of the package root). https://github.com/Polymer/polymer-bundler/issues/531
- Fixed error when generating source maps due to original line or column not being available. https://github.com/Polymer/polymer-bundler/issues/516
- Fixed `--manifest-out` option to include inlined scripts and styles and emit during single bundle cases. https://github.com/Polymer/polymer-bundler/issues/549 and https://github.com/Polymer/polymer-bundler/issues/550

## 2.0.3 - 2017-06-07
- Fixed issue with lazy-imports impacting bundle inlining logic. https://github.com/Polymer/polymer-bundler/issues/536
- Fixed issue with hidden div being created inside dom-modules. https://github.com/Polymer/polymer-bundler/issues/535
- Fixed issue with injected dependencies inside dom-modules in shell when using lazy-imports. https://github.com/Polymer/polymer-bundler/issues/534

## 2.0.2 - 2017-06-02
- In cases such as the shell merge strategy, imports which are not originally
  part of the shell are now inserted before other imports which are dependent
  on them.  Fixes https://github.com/Polymer/polymer-bundler/issues/517.

## 2.0.1 - 2017-05-19
- Don't include the `by-polymer-bundler` hidden div in bundled files
  if it is empty.

## 2.0.0 - 2017-05-18
- Official 2.0.0 release of polymer-bundler! 🎉
- Fixed a bug with url-rewriting in cases where base urls provided
  has mix of relative and absolute pathnames.

## 2.0.0-pre.18 - 2017-05-15
- Updated dependency on `polymer-analyzer` to ^2.0.0 now that it is
  out of alpha.

## 2.0.0-pre.17 - 2017-05-11
- BREAKING: Bundler's `bundle()` method now returns a `BundleResult`
  instead of just a document collection. It does this to include a
  copy of the provided manifest, where each bundle includes a record
  of the sets of imports which were inlined during bundling.

## 2.0.0-pre.16 - 2017-05-10
- Fixed issue where an inlined import's `<link rel="lazy-import">` href
  was being adjusted, but should not be adjusted when governed by the
  containing dom-module's assetpath.

## 2.0.0-pre.15 - 2017-05-08
- Fixed case where an inlined import's `<link rel="lazy-import">` tags
  were being moved.
- Updated `polymer-analyzer` to latest version.

## 2.0.0-pre.14 - 2017-05-03
- BREAKING: Bundler options now include `strategy` and `urlMapper`.  These
  have been moved out from the `generateManifest` method which now only
  takes the `entrypoints` array.  If you had code that used this method,
  you'll need to move those parameters to your `new Bundler({...})` call.
- Added new url mapper functions to make customizing destination of shared
  bundle files easier: `generateSharedBundleUrlMapper` and
  `generateCountingSharedBundleUrlMapper` in `src/build-manifest`.
- Fixed an issue where `<link rel="lazy-import">` tags were being moved
  out of their containing `<dom-module>` tags.
- Export all of the bundle manifest types and functions on the default
  namespace of `polymer-bundler`.

## 2.0.0-pre.13 - 2017-05-01
- BREAKING: Bundler now supports "lazy imports" aka `<link rel="lazy-import">`.
  URLs which are imported lazily are implicitly treated as entrypoints.  This
  is a breaking change because, previously, Bundler would incorrectly treat
  lazy imports just like regular imports.
- BREAKING: Bundler now inlines Scripts and CSS by default.  Pass `inlineCss`
  and `inlineScripts` as `false` explicitly to disable.
- Fixed an issue where `exclude` option did not actually support folder names.

## 2.0.0-pre.12 - 2017-04-14
- BREAKING: Public API change.  The `bundle()` method now takes a manifest
  instead of entrypoints, strategy and mapper.  To produce a manifest,
  use new public method `generateManifest()`.
- The polymer-bundler CLI now uses the current working directory as
  the package root folder for its Analyzer, allowing absolute paths to
  resolve properly.
- Fixed an issue where an immediate `<style>` child of `<dom-module>` was
  not moved into generated `<template>`.
- BREAKING: Added option `rewriteUrlsInTemplates` to support rewriting of urls
  found in `src`, `href`, `action`, `assetpath` and `style` attributes
  found inside `<template>` elements, when inlining html imports.  Previously,
  this was done by default.  Now requires explicit option.
- BREAKING: inlinining functions in `import-utils` require an explicit
 `analyzer` argument.
- BREAKING: Dropped support for node v4, added support for node v8. See our
  [node version support policy]
  (https://www.polymer-project.org/2.0/docs/tools/node-support) for details.
- Fixed an issue where bundler was adding `<html>`, `<head>`, and `<body>` tags
  to documents that didn't have them.
- Removed unused/not-implemented options from the polymer-bundler CLI and
  corresponding `Bundler` options: `strip-exclude`, `no-implicit-strip` and
  `redirect`.
- Corrected the CLI usage output.
- Corrected the README.

## 2.0.0-pre.11 - 2017-03-20
- Bump dependency on analyzer

## 2.0.0-pre.10 - 2017-03-15
- Add a sourcemap option to properly handle or create sourcemaps for
  script tags

## 2.0.0-pre.9 - 2017-03-07
- Bump dependency on analyzer

## 2.0.0-pre.8 - 2017-03-03
- Uses `polymer-analyzer` directly to obtain import document sources and
  ASTs.  This enforces agreement with the Analyzer as to which documents
  should be inlined/bundled.
- Fixes issue where protocol-less URLs (those that start with `//`) were
  treated as absolute paths.
- Renamed/refactored to draw a clearer distinction between ASTs and
  documents.
- Fixes issue where absolute paths in urls would be rewritten in unexpected
  ways because they were not resolved by the same url resolver rules used
  by the Analyzer.

## 2.0.0-pre.7 - 2017-03-03
- Bump dependency on analyzer.

## 2.0.0-pre.6 - 2017-02-17
- Handle `<base href="...">` values correctly when inlining imports.
- Apply `<base target="...">` to links and forms in the same document as
  appropriate.
- License comments are deduplicated properly now.
- Server-Side Include comments `<!--# ... -->` are no longer stripped.
- Fixed a bug where element attributes inside `<template>` tags were not
  being rewritten when html imports were inlined.

## 2.0.0-pre.5 - 2017-02-14
- Handle base tags when resolving the dependency graph of imports.

## 2.0.0-pre.4 - 2017-02-06
- Fixed a bug where bundling using generateShellMergeStrategy would result in
  builds missing imports which should have been appended to shell.

## 2.0.0-pre.3 - 2017-01-31
- Changed the way bundle documents are generated, so that bundles based on a
  source file retain their HTML structure instead of always being synthesized
  from a blank document.
- bundle() is the only public method on Bundler class now.
- Bundler does not explicitly throw an error if it spots a `<polymer-element>`
  somewhere in a document.
- `--add-import` and `--importUrl` options have been removed.  These features,
  as expressed, need to be conceptually reworked to work in bundler's new
  multi-bundle paradigm.
- `--abspath`/`basePath` have been removed, anticipating their reimplementation
  in `polymer-build`.  https://github.com/polymer/polymer-build/issues/117

## 2.0.0-pre.2 - 2017-01-17
- Minor API difference the way Bundler class is exported to make it import
  friendly.
- Reduced set of files in published package.

## 2.0.0-pre.1 - 2017-01-17
- Complete rewrite of codebase in Typescript and changed from hydrolysis to
  polymer-analyzer.
- Built-in support for build sharding.
- Optionally generates a manifest of each bundle's imports.

## 1.16.0 - 2017-07-14
- Fix erroneously reverse-order inlining of style imports `<link rel="import" type="css">`.
- Added `--polymer2` flag that changes some of the rewriting behaviors to support the fact that Polymer 2.x honors the `assetpath` of `<dom-module>` when interpreting style urls:
  - Disables rewriting urls in inlined html imports containing `<style>` tags inside `<dom-module>` containers.
  - Include consideration of container `<dom-module>` `assetpath` property when rewriting urls inside inlined style imports.

## 1.15.5 - 2017-06-01
- Add --out-request-list to bin/vulcanize help message

## 1.15.4 - 2017-03-21
- `excludes` option now honors JavaScript asset references:
  - Won't attempt to load the JS (which caused errors when local file not present.)
  - Won't inline excluded JS files.
- Added --out-request-list option, which writes a list of request URLs required
  to vulcanize `<html file>` to a given file on success.

## 1.15.3 - 2017-01-17
- Fix for how paths are rewritten in nested import scenarios where paths to same
  directory were ignored instead of treated as "." for relative path.

## 1.15.2 - 2016-12-16
- Fix for how assetpath is set when in the same directory, where assetpath attribute
  for components in same directly were incorrectly set to "/".

## 1.15.1 - 2016-12-16
- Amended README and minor text fixes to CHANGELOG post release.

## 1.15.0 - 2016-12-16
- Fixed: Preserve style ordering when moving imports and links out of head tag.
- Don't actually move anything out of head until an html import is encountered.

## 1.14.12 - 2016-11-16
- Removed update-notifier dependency and functionality
- Fixed: Stopped moving links with certain rels out of the head tag to avoid breaking
  favicons, manifests, and other such features.
- Speed enhancements

## 1.14.11 - 2016-04-11
- Radically simpler inlining design. Move all imports and scripts into document
  body, replace imports inline.
- Add tests for more complicated inlining scenarios

## 1.14.10 - 2016-04-07
- Must use path resolution when moving `<head>` nodes

## 1.14.9 - 2016-04-05
- Try harder to preserve script ordering, move nodes from `<head>` of current
  document into import inlining fragment

## 1.14.8 - 2016-03-24
- Undo hydrolysis inlining scripts

## 1.14.7 - 2016-03-01
- Fix inlining for firebase and other scripts that use `\\x3c/script` syntax
  when making more scripts

## 1.14.6 - 2016-02-23
- Make sure `@license` comments always are maintained

## 1.14.5 - 2016-01-21
- Fix dom5 dependency to 1.3+

## 1.14.4 - 2016-01-21
- Make sure `<link rel="import type="css">` inlining is placed into a
  `<dom-module>`'s `<template>`

## 1.14.3 - 2016-01-19
- Fix for trailing slash in `<base>` tag

## 1.14.2 - 2016-01-19
- Fix paths when preserving execution order moves scripts to body

## 1.14.1 - 2016-01-14
- Escape inline scripts
- Strip Excludes fixed to have higher precedence than Excludes
- Fix script execution order with imports, once and for all!

## 1.14.0 - 2015-10-20
- Add `output` option to write file from CLI

## 1.13.1
- Strip Excludes should be fuzzy

## 1.13.0
- Support custom hydrolysis file loaders

## 1.12.3
- Make sure `excludes` works with `redirects`

## 1.12.2
- Fix CLI spelling of `redirects`

## 1.12.1
- Fix misspelling of `redirects` in library options

## 1.12.0
- New `--redirect` flag and `redirect` argument to set up custom path mappings

## 1.11.0
- New `-add-import` flag and `addedImports` argument to add additional imports
    to the target file.
- Copy `@media` queries on external CSS files into inlined styles.
- Fix excluding css files from computing dependencies.

## 1.10.5
- Fix a dumb unix path assumption for --inline-scripts and --inline-css +
    absolute paths on windows.

## 1.10.4
- Fix excludes for js files and folders

## 1.10.3
- Fix regression in --strip-comments from 1.9.0

## 1.10.2
- Use URLs internally until calling hydrolysis
- Fixes a bunch of inline issues for Windows

## 1.10.1
- Typecheck inputs in library usage
- Fix README to say that `stripExcludes` is an Array not a Boolean

## 1.10.0
- Add `inputUrl` option to work around grunt and gulp plugins providing
  filepaths that cannot be used as URLs to `vulcanize.process()`

## 1.9.3
- Fix abspath bug on windows machines

## 1.9.2
- Use new class API in binary
- Update dependencies

## 1.9.1
- Fix `implicitStrip` in new Class based API

## 1.9.0
- New class based API:
```js
var Vulcanize = require('vulcanize');

var vulcan = new Vulcanize(options);
vulcan.process(...);
```
- `vulcanize.setOptions` and `vulcanize.process` are deprecated
## 1.8.1
- Bump hydrolysis to 1.12.0 with proper ordering

## 1.8.0
- Make stripComments work more reliably

## 1.7.1
- Don't try to inline styles from external sources

## 1.7.0
- Inline link[rel="stylesheet"] css as well as polymer import stylesheets

## 1.6.0
- Update usage of private API of hydrolysis
- Correctly set 'implicit strip' option when used programatically

## 1.5.1
- Ignore external (http and https) resources from inlining

## 1.5.0
- Error on the use of old Polymer elements. Vulcanize 0.7.x is the last version
    that will handle &lt; Polymer 0.8.
- Rewrite urls for inlined styles

## 1.4.4
- Make sure excluded js files are totally removed (they inserted blank script
    tags)

## 1.4.3
- Update dependencies and docs
- Dependency update fixes cyclic dependencies

## 1.4.2
- Fix URL rewriting from parts of imports that end up in `<body>`

## 1.4.1
- `--implicit-strip` is default
- Remove "comment normalization" when stripping, it was not self-stable

## 1.4.0
- Add `--strip-comments` to remove unnecessary comments

## 1.3.0
- Add `--inline-css` option to inline external stylesheets

## 1.2.1
- Update dependencies

## 1.2.0
- Change `--strip-exclude` to be an array of excludes to strip
- `--implicit-strip` is the old `--strip-excludes` behavior

## 1.1.0
- Add `--inline-scripts` option to inline external scripts

## 1.0.0
- Rewrite on top of [hydrolysis](https://github.com/PolymerLabs/hydrolysis) and
[dom5](https://github.com/PolymerLabs/dom5)
- Factor out `--csp` flag into [crisper](https://github.com/PolymerLabs/crisper)
- Remove html and javascript minification

## 0.7.10
- Collapse whitespace instead of removing it
- Keep unique license comments

## 0.7.9
- Honor `<base>` urls in inline styles

## 0.7.8
- Update to whacko 0.17.3

## 0.7.7
- Honor `<base>` tag
- Make all schemas "absolute" urls

## 0.7.6
- Don't rewrite urls starting with '#'

## 0.7.5
- Remove cssom, just use regexes

## 0.7.4
- Workaround for cssom not liking '{{ }}' bindings in `<style>` tags (unsupported, use `<core-style>` instead)

## 0.7.3
- Replace clean-css with cssom, which does less "optimizations"

## 0.7.2
- Disable css number rounding for crazy-sad flexbox hacks in IE 10
- Add charset=utf-8 to all scripts
- Better comment removal codepath

## 0.7.1
- Support for mobile URL Schemes "tel:" and "sms:"
- Better reporting of javascript error messages with `--strip`
- Handle buffers as input with `inputSrc`
- Rename `outputSrc` to `outputHandler`

## 0.7.0
- Upgrade to whacko 0.17.2 with template support
- add utils.searchAll to make a query that walks into `<template>` elements

## 0.6.2
- stick to whacko 0.17.1 until `<template>` support is complete

## 0.6.1
- fix bug with removing absolute imports

## 0.6.0
- Strip excluded imports by default (old behavior accessible with --no-strip-excludes flag)

## 0.5.0
- finally switch to new-world polymer license
- Add a bunch of tests for lib/vulcan
- Refactor test suites
- tests for utils and optparser modules
- Merge pull request #83 from jongeho1/undefined-element
- undefined element fix
- remove unnecessary require statement
- Handle indirect prototype references in Polymer invocation
- plumb abspath to all url rewriting
- shields!
- add travis config
- add tests!
- Add option for printing version and nag to update
- move test folder to example
- Merge branch 'master' of github.com:rush340/vulcanize into rush340-master
- Merge pull request #75 from ragingwind/remove-importerjs
- Merge pull request #77 from Polymer/use-whacko
- Keep consistent ordering of import document heads and bodies
- Don't create a whole document for inlining styles
- Switch to whacko/parse5
- fix flipped conditional
- Merge pull request #76 from ragingwind/buffer
- Support buffer in/out
- Remove importer.js
- more explicit checking of whether abspath is set
- cleaned up regex matching of root
- renamed webAbsPath to abspath
- fixed cheerio options to perform the same parsing while reading and writing
- if webAbsPath is passed in, use absolute paths everywhere
- resolve webAbsPath if relative path provided
- added recognition of double-slash paths as a remote absolute URL
- applied webAbsPath option for handling absolute paths (based on jongeho1's pull request: https://github.com/Polymer/vulcanize/pull/36)

## 0.4.3
- Release 0.4.3
- Mailto: is an absolute path
- Merge pull request #70 from rush340/htmlentities
- added missing use of CHEERIO_OPTIONS
- fixed cheerio options to perform the same parsing while reading and writing
- Merge pull request #59 from mozilla-appmaker/cheerio-write-fix
- Merge pull request #65 from tbuckley/patch-1
- Add quotes around filenames in CSS
- audit license headers
- fixed cheerio options to perform the same parsing while reading and writing
- Never decode entities

## 0.4.2
- Fix inline svgs
- Update README with --strip functionality

## 0.4.1
- Bump version to 0.4.1
- Strip comments and whitespace from all nodes

## 0.4.0
- Bump to version 0.4.0
- Replace noscript with explicit Polymer invocation, to ensure correct element registration order when CSP'ed.

## 0.3.1
- remove extraneous async module
- Fixes #34

## 0.3.0
- Hide import content from view in the main document

## 0.2.7
- always add name to polymer invocation

## 0.2.6
- bump version
- add small usage block to help
- Make --strip work with --csp
- Clean up use of get/setTextContent
- Inline stylesheet happens after import path fixup, so outputPath of rewriteURL should be the overall outputPath

## 0.2.5
- update to 0.2.5
- .text() was decoding HTML entities, read raw script node content for CSP
- Support Polymer invocation without tag name
- Fix slightly broken merge conflict
- Enable `--inline --csp` mode to smash everything into one JS file
- Upstream cheerio changed loop semantics to return "dom" nodes instead of sugared cheerio objects
- Fix #29
- Print help dialog if called without arguments
- update dependencies

## 0.2.4
- Treat config file as "defaults", commandline flags override
- Do path resolution before import processing and style inlining

## 0.2.3
- A few bug fixes

## 0.2.2
- Don't recalculate assetpath for handled elements
- Bump to 0.2.1

## 0.2.1
- unbreak assetpath generation

## 0.2.0
- Prepare vulcanize 0.2.0
- Merge pull request #25 from lborgav/patch-1
- Fixing missing letters
- Don't move external scripts around with CSP mode
- Use uglify inline_script
- Use cleancss only for stripping comments
- Merge pull request #21 from azakus/modular
- went a little too quick with the regex
- Remove byte order mark
- Make sure not to lose assetpath fix
- First draft at a split out Importer
- Inplace inline *all* imports
- Copy setTextNode since it's so tiny
- move all the option validation into optparser
- Update npm dependencies
- Split out path resolution
- Break out option parser
- Break out constants
- Add the hooks for style and script excludes
- Add changelog generation script
- Merge pull request #16 from tbuckley/master
- Include excluded script instead of its contents
- Only put a trailing slash into assetpath attribute if there is a path
- bump version
- clone all styles (minus href and rel) from `<link>` to `<style>`
- update to 0.1.13
- Skip non-JS scripts and non-CSS styles
- bump version
- Make sure to CSPify main document first, load platform.js first in the output js file.
- add test config for excluding polymer.html
- Refactor handling of inlined and excluded import insertion
- bump version
- Fix subtle path bug in stylesheets
- use uglify and clean-css to strip comments from js and css when using --strip
- Clean up
- bump version
- --csp will now operate on the input html file as well
- Fix script inlining to ignore parsing html comments
- cheerio 0.13 seems to work just fine
- inline stylesheets in the main page when using --inline
- README: add ga beacon

## 0.1.9
- Reset excludes on each run

## 0.1.8
- Bump version
- add "strip comments" functionality
- fix minor typo in helep text: s/defualts/defaults

## 0.1.7
- bump version
- add sub-import test to the top level import
- Add --config option to specify user defined excludes
- Add user-defined excludes from inling.

## 0.1.6
- bump version
- test with absolute urls
- remove console.log
- Deduplicate absolute url imports
- fix missing absolute imports

## 0.1.5
- bump to 0.1.5
- Revert "polymer-scope is no longer supported"

## 0.1.4
- reset shared buffers on each handleMainDocument call

## 0.1.3
- bump version
- move option checking to setOptions, not the bin
- Add npm installation instructions
- polymer-scope is no longer supported

## 0.1.2

## 0.1.15
- Only put a trailing slash into assetpath attribute if there is a path

## 0.1.14
- bump version
- clone all styles (minus href and rel) from `<link>` to `<style>`

## 0.1.13
- update to 0.1.13
- Skip non-JS scripts and non-CSS styles

## 0.1.12
- bump version
- Make sure to CSPify main document first, load platform.js first in the output js file.
- add test config for excluding polymer.html
- Refactor handling of inlined and excluded import insertion

## 0.1.11
- bump version
- Fix subtle path bug in stylesheets
- use uglify and clean-css to strip comments from js and css when using --strip
- Clean up

## 0.1.10
- bump version
- --csp will now operate on the input html file as well
- Fix script inlining to ignore parsing html comments
- cheerio 0.13 seems to work just fine
- inline stylesheets in the main page when using --inline
- README: add ga beacon
- Reset excludes on each run
- Bump version
- add "strip comments" functionality
- fix minor typo in helep text: s/defualts/defaults
- bump version
- add sub-import test to the top level import
- Add --config option to specify user defined excludes
- Add user-defined excludes from inling.
- bump version
- test with absolute urls
- remove console.log
- Deduplicate absolute url imports
- fix missing absolute imports
- bump to 0.1.5
- Revert "polymer-scope is no longer supported"
- reset shared buffers on each handleMainDocument call
- bump version
- move option checking to setOptions, not the bin
- Add npm installation instructions
- polymer-scope is no longer supported
- bump version
- update README to be more approachable
- add a help dialog, fix "main" in package.json

## 0.1.1
- Bump version to 0.1.1
- Fix paths from main html file if input or output directories are not current working directory
- Add style url rewriting back
- add other directories to testing
- Merge pull request #3 from akhileshgupta/inline_styles_fix
- Merge pull request #2 from akhileshgupta/concat_scripts_bugfix
- variable rename and removing the unrequired check
- fixing the use of .html(cssText) to update the styles content.
- resolving script path from outputDir  during concatenation
- Merge pull request #1 from addyosmani/patch-1
- Adds npm install snippet, minor formatting changes.

## 0.1.0
- semver recommends starting at 0.1.0
- add repo info to package.json

## 0.0.1
- Update README.md
- add license top
- remove unrelated viz file
- add license files
- reference new executable path
- reference bin/vulcanize for global npm install
- split vulcan.js into vulcanize bin and lib/vulcan.js
- reorder constant variables, add missing SCRIPT_SRC
- inlineScripts now uses html text and regex, not cheerio api
- Use html() to inline scripts, text() makes HTML Entities
- Add --inline option to inline all scripts into main document (opposite of --csp)
- Update README to reflect all-in-one html files
- Try to insert inlined import exactly where the link was
- make everything from imports inlined
- update README with index-vulcanized output
- Inlined stylesheets must have URL paths rewritten, move to import processing
- inline css stylesheets into style tags in polymer elements
- assetpath is handled by polymer now
- Update README.md
- Update README.md
- Remove unused function
- fix import location finding and windows path munging
- Fix output directory for CSP js file
- find better spots for vulcanized imports and scripts
- Update to newer cheerio with fixed htmlparser
- reflect new functionality in README, fix up newline issues, refactor constants
- vulcanizer will now take in a single main document and produce a built version of that main document.
- add a semicolon to all scripts to prevent weird insertion conditions
- update README for CSP mode
- For CSP, allow an option to separate scripts into a separate file
- Process imports as whole files, no element extraction
- breaking down doc tool for analysis
- Update README for polymer-element
- update for polymer-element
- Much more useful README
- use assetpath attribute on `<element>` to fix resolvePath usage in Polymer elements
