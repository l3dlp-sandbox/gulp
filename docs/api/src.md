<!-- front-matter
id: src
title: src()
hide_title: true
sidebar_label: src()
-->

# src()

Creates a stream for reading [Vinyl][vinyl-concepts] objects from the file system.

**Note:** BOMs (byte order marks) have no purpose in UTF-8 and will be removed from UTF-8 files read by `src()`, unless disabled using the `removeBOM` option.

## Usage

```javascript
const { src, dest } = require('gulp');

function copy() {
  return src('input/*.js')
    .pipe(dest('output/'));
}

exports.copy = copy;
```


## Signature

```js
src(globs, [options])
```

### Parameters

| parameter | type | note |
|:--------------:|:------:|-------|
| globs | string<br />array | [Globs][globs-concepts] to watch on the file system. |
| options | object | Detailed in [Options][options-section] below. |

### Returns

A stream that can be used at the beginning or in the middle of a pipeline to add files based on the given globs.

### Errors

When the `globs` argument can only match one file (such as `foo/bar.js`) and no match is found, throws an error with the message, "File not found with singular glob". To suppress this error, set the `allowEmpty` option to `true`.

When an invalid glob is given in `globs`, throws an error with the message, "Invalid glob argument".

### Options

**For options that accept a function, the passed function will be called with each Vinyl object and must return a value of another listed type.**


| name | type | default | note |
|:--------:|:------:|------------|--------|
| encoding | string<br />boolean | "utf8" | When false, file contents are treated as binary. When a string, this is used as the text encoding. |
| buffer | boolean<br />function | true | When true, file contents are buffered into memory. If false, the Vinyl object's `contents` property will be a paused stream. It may not be possible to buffer the contents of large files.<br />**Note:** Plugins may not implement support for streaming contents. |
| read | boolean<br />function | true | If false, files will be not be read and their Vinyl objects won't be writable to disk via `.dest()`. |
| since | date<br />timestamp<br />function | | When set, only creates Vinyl objects for files modified since the specified time. |
| removeBOM | boolean<br />function | true | When true, removes the BOM from UTF-8 encoded files. If false, ignores a BOM. |
| sourcemaps | boolean<br />function | false | If true, enables [sourcemaps][sourcemaps-section] support on Vinyl objects created. Loads inline sourcemaps and resolves external sourcemap links. |
| resolveSymlinks | boolean<br />function | true | When true, recursively resolves symbolic links to their targets. If false, preserves the symbolic links and sets the Vinyl object's `symlink` property to the original file's path. |
| cwd | string | `process.cwd()` | The directory that will be combined with any relative path to form an absolute path. Is ignored for absolute paths. Use to avoid combining `globs` with `path.join()`.<br />_This option is passed directly to [glob-stream][glob-stream-external]._ |
| base | string | | Explicitly set the `base` property on created Vinyl objects. Detailed in [API Concepts][glob-base-concepts].<br />_This option is passed directly to [glob-stream][glob-stream-external]._ |
| cwdbase | boolean | false | If true, `cwd` and `base` options should be aligned.<br />_This option is passed directly to [glob-stream][glob-stream-external]._ |
| root | string | | The root path that `globs` are resolved against.<br />_This option is passed directly to [glob-stream][glob-stream-external]._ |
| allowEmpty | boolean | false | When false, `globs` which can only match one file (such as `foo/bar.js`) causes an error to be thrown if they don't find a match. If true, suppresses glob failures.<br />_This option is passed directly to [glob-stream][glob-stream-external]._ |
| uniqueBy | string<br />function | `'path'` | Remove duplicates from the stream by comparing the string property name or the result of the function.<br />**Note:** When using a function, the function receives the streamed data (objects containing `cwd`, `base`, `path` properties). |
| dot | boolean | false | If true, compare globs against dot files, like `.gitignore`.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| nounique | boolean | false | When false, prevents duplicate files in the result set.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| debug | boolean | false | If true, debugging information will be logged to the command line.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| nobrace | boolean | false | If true, avoids expanding brace sets - e.g. `{a,b}` or `{1..3}`.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| noglobstar | boolean | false | If true, treats double-star glob character as single-star glob character.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| noext | boolean | false | If true, avoids matching [extglob][extglob-docs] patterns - e.g. `+(ab)`.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| nocase | boolean | false | If true, performs a case-insensitive match.<br />**Note:** On case-insensitive file systems, non-magic patterns will match by default.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| matchBase | boolean | false | If true and globs don't contain any `/` characters, traverses all directories and matches that glob - e.g. `*.js` would be treated as equivalent to `**/*.js`.<br />_This option is passed directly to [anymatch][anymatch-external]._ |
| ignore | string<br />array | | Globs to exclude from matches. This option is combined with negated `globs`.<br />**Note:** These globs are always matched against dot files, regardless of any other settings.<br />_This option is passed directly to [anymatch][anymatch-external]._ |

## Sourcemaps

Sourcemap support is built directly into `src()` and `dest()`, but is disabled by default. Enable it to produce inline or external sourcemaps.

Inline sourcemaps:
```js
const { src, dest } = require('gulp');
const uglify = require('gulp-uglify');

src('input/**/*.js', { sourcemaps: true })
  .pipe(uglify())
  .pipe(dest('output/', { sourcemaps: true }));
```

External sourcemaps:
```js
const { src, dest } = require('gulp');
const uglify = require('gulp-uglify');

src('input/**/*.js', { sourcemaps: true })
  .pipe(uglify())
  .pipe(dest('output/', { sourcemaps: '.' }));
```

[sourcemaps-section]: #sourcemaps
[options-section]: #options
[vinyl-concepts]: ../api/concepts.md#vinyl
[glob-base-concepts]: ../api/concepts.md#glob-base
[globs-concepts]: ../api/concepts.md#globs
[extglob-docs]: ../documentation-missing.md
[anymatch-external]: https://github.com/micromatch/anymatch
[glob-stream-external]: https://github.com/gulpjs/glob-stream
