{
  title: "Write grunt usage",
  date:   "2014-06-15",
  description: "Writing grunt usage is kindness"
}
## Why it sould write grunt usage?
Everytime reading Gruntfile.js spend many time when I'm gonna contribute something kind of like a front end project. So I proposed this:
```
grunt.registerTask('default', function () {
  console.info('See grunt usage');
});

grunt.registerTask('usage', function () {
  var message = '';
  message += 'Usage:\n';
  message += '    grunt usage:  Show this message\n';
  message += '    grunt dist :  Build dist source\n';
  message += '    grunt watch:  Watch src/*.js, run dist';

  console.info(message);
});
```


Thanks.
