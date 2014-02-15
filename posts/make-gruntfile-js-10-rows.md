{
  title: "Make Gruntfile.js 10 rows",
  date:   "2014-02-15",
  description: "The story is that I reduce the number of rows in Gruntfile.js"
}
## Background
Everyone loves Grunt.js that is highly functional JavaScript task runner and we expect too much on Gruntfile.js as is often the case like the following:

+ Concatenate files
+ Minify files with UglifyJS
+ Validate files with JSHint
+ Compile CoffeeScript files to JavaScript
+ Run Nodeunit unit tests
+ Compile underscore templates to JST file.
+ Start a static web server.
+ etc...

If all of these tasks are putted to Gruntfile.js then the file line of row become more than 300 line. Furthermore, it was added some tasks on a per-service basis with some append the path to the directory then the number of rows will increase tripled twice.
Using `require` for avoid this problem just because we are using node.js.

## Example code
### Gruntfile.js
Gruntfile.js is only for:

+ Get and Set object for initConfig
+ Load NpmTask
+ Register gruntTask

```javascript
// Gruntfile.js
module.exports = function(grunt) {
  'use strict';
  var configObject = require('./grunt/config');
  var PACKAGE_JSON = grunt.file.readJSON("package.json");
  grunt.config.init(configObject);
  // Load Grunt Plugins
  Object.keys(PACKAGE_JSON.devDependencies).slice(1).forEach(grunt.loadNpmTasks);
  grunt.registerTask('default', ['connect', 'watch']);
  grunt.registerTask('release', ['less', 'jst', 'concat', 'jshint', 'uglify']);
};
```

### config.js
+ Interface of initConfig

```javascript
// grunt/config.js
module.exports = {
  connect: require('./connect'),
  htmlmin: require('./htmlmin'),
  jst: require('./jst'),
  jshint: require('./jshint'),
  concat: require('./concat'),
  uglify: require('./uglify'),
  watch: require('./watch')
};
```

### Other config(s)
+ 1 File per 1 Module

```javascript
// grunt/connect.js
module.exports = {
  server: {
    options: {
      port: 3000,
      base: 'www/'
    }
  }
};
```

# Impre
TG there's `module.exports` <3

A project using that: [dartstack](https://github.com/watilde/dartstack)
