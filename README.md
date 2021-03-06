# gulp-grunt
#### Run grunt tasks from gulp
[![travis build](https://api.travis-ci.org/gratimax/gulp-grunt.png)](https://travis-ci.org/gratimax/gulp-grunt)
[![dependencies](https://david-dm.org/gratimax/gulp-grunt.png)](https://david-dm.org/gratimax/gulp-grunt)
[![dev dependencies](https://david-dm.org/gratimax/gulp-grunt/dev-status.png)](https://david-dm.org/gratimax/gulp-grunt#badge-embed=&info=devDependencies&view=table)

What if your favorite grunt plugin isn't available for gulp yet? No worries! Just hook in your
grunt configuration!
This plugin is a bit different from most other gulp plugins.
You cannot use it inline, because it does not create a stream.
Rather, use it at the top of your gulpfile, calling it with your gulp as an argument.

## Example usage
```js
var gulp = require('gulp')
require('gulp-grunt')(gulp) // add all the gruntfile tasks to gulp

// continue defining tasks...
gulp.task('do-this', function() {
    ...
})

// run them like any other task
gulp.task('default', function() {
    gulp.run('grunt-minify')
    gulp.run('grunt-test')
})
```
Note that all the grunt tasks that were added begin with the prefix 'grunt-'.
This is for usability, so that your grunt tasks do not clash with your gulp tasks.
Also note that `require('gulp-grunt')(gulp)` does not have to be at the top of your file.
It could very well be at the bottom, except that then it could possibly overwrite some of your
gulp tasks.

## Functions

### gulp-grunt()
__Takes__ `(gulp, options)`

Configuration is done with the function call:
```js
require('gulp-grunt')(gulp, {
    base: ...,
    prefix: ...
})
```
This function appends all the grunt tasks it has found to your gulp object as normal gulp tasks.

#### gulp
Your gulp object that you imported with the code:
```js
var gulp = require('gulp')
```
Pass it in and gulp-grunt will add all the tasks.

#### options
`options` is the configuration object you pass in.

#### options.base
This tells grunt where to look for your gruntfile.
Set it to some absolute path.
This may require you to use `path.join` for relative paths:
```js
require('gulp-grunt')(gulp, {
    base: require('path').join(__dirname, 'yourrelativepathhere')
})
```

#### options.prefix
This tells gulp-grunt how to prefix your tasks.
For instance, if in the gruntfile you define the tasks 'minify' and 'compile',
and if you pass gulp-grunt this configuration:
```js
require('gulp-grunt')(gulp, {
    prefix: 'theknightswhosay-'
})
```
The grunt tasks can be called from gulp, except they would have the prefix, so
'theknightswhosay-minify' and 'theknightswhosay-compile'.

You can simply pass in an empty string(`''`) if you wish to have no prefix.

#### default options

```js
{
    base: null, // this is just the directory that your Gulpfile is in
    prefix: 'grunt-'
}
```

### gulp-grunt.tasks()
__Takes__ `(options)`

This just returns all the grunt tasks found, along with their associated functions.
Calling is essentially the same as with the main function:
```js
var gulp_grunt = require('gulp-grunt')
var tasks = gulp_grunt.tasks({
    base: ...,
    prefix: ...
})
```
Output is something like:
```js
{
    'grunt-test': [Function],
    'grunt-minify': [Function]
    // etc...
}
```

#### options
This object is the exact same as for [gulp-grunt()](#gulp-grunt-1) above. 
This tells gulp-grunt what prefix to use and what base to search for.

***

Have fun grunting and gulping! :D