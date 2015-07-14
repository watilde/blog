title: "npm-3-troubled-case"
date: 2015-07-15 16:57:43
tags:npm
---
This article has been translated from [this article](http://qiita.com/mizchi/items/a398cd49941e21f65d85), original article was written by [@mizchi](https://github.com/mizchi).

---
I just have run `npm install -g npm@3` and trying to use it today. 3.1.2 seems `VERY BETA`(I don't know the actual meaning of this whether close or far from alpha), it will be major release at some future time.

## a flatter node_modules directory
All of your dependencies will be sitting next to each other at the top level. Only when there are conflicts will modules be installed at deeper levels. This make things data volume reduction when there are conflicts.

## "require" hasn't changed at all
Now it has become possible to require directly not only child module but also grandchild. For example, when use foo module depended on bar module, it has become possible to do `require('bar')`. This means the possibility of rewritten dynamically a module required. I concerned about this problem.

## An actual example
It required same module by cache when use same version between child and grandchild. From a standpoint of child module, set global option when child module assume grand child module as singleton, it suddenly has broken because of flat directory.

e.g. To use `marked` module

gfm-marked/gfm-marked.js
```js
var marked = require('marked');
marked.setOptions({
  renderer: new marked.Renderer(),
  gfm: true
});
module.exports = marked
```

not-gfm-marked/not-gfm-marked.js
```js
var marked = require('marked');
marked.setOptions({
  renderer: new marked.Renderer(),
  gfm: false
});
module.exports = marked
```

index.js
```js
var notGfm = require('not-gfm-marked');
var gfm = require('gfm-marked');

// There is no fenced code block,
// but it become valid by the order required
notGfm('```hello```');
```

It seems very rare case, but it might be so hard to resolve it. The other side, browserify doesn't support npm@3 yet. We need to wait a little.

Thanks.

## See also
+ [npm Weekly, #5](http://blog.npmjs.org/post/110924823920/npm-weekly-5
)
+ [npm v3.x 試してみた & 注意する点](http://qiita.com/mizchi/items/a398cd49941e21f65d85)
