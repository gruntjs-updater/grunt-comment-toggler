# grunt-comment-toggler

> Toggle line comments inside build blocks.

## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out
the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains
how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as
install and use Grunt plugins. Once you're familiar with that process, you may
install this plugin with this command:

```shell
npm install grunt-comment-toggler --save-dev
```

## The "toggleComments" task
`toggleComments` searches files for [build blocks](#build-blocks) and comments
or uncomments each line inside the block depending on the action. The delimiter
used by the comments is also specified in the build block.

### Build blocks
Build blocks are defined using the following syntax:
```
<!-- comments:(action) (comment character) -->
Your code goes here
<!-- endcomments -->
```
- `action` is the type of processing done for each line. Valid actions are
`comment`, `uncomment` and `toggle`.
- `comment character` is the delimiter used for comments. Any sequence of
non-whitespace characters is valid. There are two
[special delimiters](#special-delimiters) that can be used.

#### Special delimiters
For `HTML` and `CSS` specifying a single delimiter for comments isn't enough.
For this purpose there are two special delimiters `html` and `css` that use
appropriate block comments to process each line. See [examples](#examples) on
how to use them.

### Options
#### padding
Type: `Number`  
Default: `1`

Amount of whitespace between the comment delimiters and the line content.

#### removeCommands
Type: `Boolean`  
Default: `false`

When `true`, removes the `<!-- comments:... -->` and `<!-- endcomments -->`
definitions from the file during processing. Can be used to prevent conflicts
with other Grunt plugins that use similar build block syntax.

### Examples
#### Basic uncommenting example
Example using a single character to uncomment line
```apache
# <!-- comments:uncomment # -->
# AddHandler application/x-httpd-php54 .php
# <!-- endcomments -->
```

#### HTML & CSS Example
How to process HTML and CSS block comments using
[special delimiters](#special-delimiters)
```html
<style>
    /* <!-- comments:toggle css --> */
    p {color: red}
    /* p {color: blue} */
    /* <!-- endcomments --> */
    
    /* The code above will swap the color from red to blue */
</style>

<!-- comments:comment html -->
<p>This line will be commented</p>
<!-- endcomments -->
```

#### Custom options example
This example shows the usage of custom options as well as a sample output.

*Gruntfile.js*
```js
toggleComments: {
    customOptions: {
        options: {
            padding: 4,
            removeCommands: true
        },
        files: {"targetfile.js": "testfile.js"}
    }
}
```

*testfile.js*
```js
// <!-- comments:comment // -->
var txt = "This will be commented with the padding";
// var txt2 = "This line will not have padding, since it's already commented";
// <!-- endcomments -->
var txt3 = "This is not affected";
```

Outputs the following in *targetfile.js*
```js

//    var txt = "This will be commented with the padding";
// var txt2 = "This line will not have padding, since it's already commented";

var txt3 = "This is not affected";
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style.
Add unit tests for any new or changed functionality. Lint and test your code
using [Grunt](http://gruntjs.com/).

## Changelog
### 0.1.0 - 2014-06-06
- Initial release