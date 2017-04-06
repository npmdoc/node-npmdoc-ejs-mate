# api documentation for  [ejs-mate (v2.3.0)](https://github.com/JacksonTian/ejs-mate)  [![npm package](https://img.shields.io/npm/v/npmdoc-ejs-mate.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-ejs-mate) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-ejs-mate.svg)](https://travis-ci.org/npmdoc/node-npmdoc-ejs-mate)
#### Express 4.x locals for layout, partial.

[![NPM](https://nodei.co/npm/ejs-mate.png?downloads=true)](https://www.npmjs.com/package/ejs-mate)

[![apidoc](https://npmdoc.github.io/node-npmdoc-ejs-mate/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-ejs-mate_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-ejs-mate/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-ejs-mate/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-ejs-mate/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Jackson Tian",
        "email": "http://weibo.com/shyvo"
    },
    "bugs": {
        "url": "https://github.com/JacksonTian/ejs-mate/issues"
    },
    "contributors": [
        {
            "name": "Jackson Tian",
            "email": "http://weibo.com/shyvo"
        },
        {
            "name": "Robert Sköld",
            "email": "robert@publicclass.se",
            "url": "http://publicclass.se"
        },
        {
            "name": "Tom Carden",
            "email": "tom@tom-carden.co.uk"
        }
    ],
    "dependencies": {
        "ejs": "1.0.0"
    },
    "description": "Express 4.x locals for layout, partial.",
    "devDependencies": {
        "coveralls": "*",
        "express": "~4.10.0",
        "istanbul": "*",
        "methods": "*",
        "mocha": "*",
        "mocha-lcov-reporter": "*",
        "should": "~3.0.0",
        "supertest": "*",
        "travis-cov": "*"
    },
    "directories": {
        "example": "example",
        "test": "test"
    },
    "dist": {
        "shasum": "1b8b8fea7350d9482e9e4bbe625c4a97fd8d9bcf",
        "tarball": "https://registry.npmjs.org/ejs-mate/-/ejs-mate-2.3.0.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "gitHead": "21c6ddf57ffe646f70e28f5e9a19e4cae2650ae2",
    "homepage": "https://github.com/JacksonTian/ejs-mate",
    "keywords": [
        "ejs",
        "layout",
        "partial"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "jacksontian",
            "email": "shyvo1987@gmail.com"
        }
    ],
    "name": "ejs-mate",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/JacksonTian/ejs-mate.git"
    },
    "scripts": {
        "test": "make test"
    },
    "version": "2.3.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module ejs-mate](#apidoc.module.ejs-mate)
1.  [function <span class="apidocSignatureSpan">ejs-mate.</span>block (name, html)](#apidoc.element.ejs-mate.block)
1.  [function <span class="apidocSignatureSpan">ejs-mate.</span>compile (file, options, cb)](#apidoc.element.ejs-mate.compile)
1.  [function <span class="apidocSignatureSpan">ejs-mate.</span>layout (view)](#apidoc.element.ejs-mate.layout)
1.  [function <span class="apidocSignatureSpan">ejs-mate.</span>partial (view, options)](#apidoc.element.ejs-mate.partial)



# <a name="apidoc.module.ejs-mate"></a>[module ejs-mate](#apidoc.module.ejs-mate)

#### <a name="apidoc.element.ejs-mate.block"></a>[function <span class="apidocSignatureSpan">ejs-mate.</span>block (name, html)](#apidoc.element.ejs-mate.block)
- description and source-code
```javascript
function block(name, html) {
// bound to the blocks object in renderFile
  var blk = this[name];
  if (!blk) {
// always create, so if we request a
// non-existent block we'll get a new one
    blk = this[name] = new Block();
  }
  if (html) {
    blk.append(html);
  }
  return blk;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.ejs-mate.compile"></a>[function <span class="apidocSignatureSpan">ejs-mate.</span>compile (file, options, cb)](#apidoc.element.ejs-mate.compile)
- description and source-code
```javascript
function compile(file, options, cb) {

  // Express used to set options.locals for us, but now we do it ourselves
  // (EJS does some __proto__ magic to expose these funcs/values in the template)
  if (!options.locals) {
    options.locals = {};
  }

  if (!options.locals.blocks) {
    // one set of blocks no matter how often we recurse
    var blocks = {};
    options.locals.blocks = blocks;
    options.locals.block = block.bind(blocks);
  }

  // override locals for layout/partial bound to current options
  options.locals.layout  = layout.bind(options);
  options.locals.partial = partial.bind(options);

  try {
    var fn = ejs.compile(file, options)
    cb(null, fn.toString());
  } catch(ex) {
    cb(ex);
  }
}
```
- example usage
```shell
...
  }

  // override locals for layout/partial bound to current options
  options.locals.layout  = layout.bind(options);
  options.locals.partial = partial.bind(options);

  try {
    var fn = ejs.compile(file, options)
    cb(null, fn.toString());
  } catch(ex) {
    cb(ex);
  }
}

function renderFile(file, options, fn){
...
```

#### <a name="apidoc.element.ejs-mate.layout"></a>[function <span class="apidocSignatureSpan">ejs-mate.</span>layout (view)](#apidoc.element.ejs-mate.layout)
- description and source-code
```javascript
function layout(view){
  this.locals._layoutFile = view;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.ejs-mate.partial"></a>[function <span class="apidocSignatureSpan">ejs-mate.</span>partial (view, options)](#apidoc.element.ejs-mate.partial)
- description and source-code
```javascript
function partial(view, options){

  var collection
    , object
    , locals
    , name;

  // parse options
  if( options ){
    // collection
    if( options.collection ){
      collection = options.collection;
      delete options.collection;
    } else if( 'length' in options ){
      collection = options;
      options = {};
    }

    // locals
    if( options.locals ){
      locals = options.locals;
      delete options.locals;
    }

    // object
    if( 'Object' != options.constructor.name ){
      object = options;
      options = {};
    } else if( options.object !== undefined ){
      object = options.object;
      delete options.object;
    }
  } else {
    options = {};
  }

  // merge locals into options
  if( locals )
    options.__proto__ = locals;

  // merge app locals into options
  for(var k in this)
    options[k] = options[k] || this[k];

  // extract object name from view
  name = options.as || resolveObjectName(view);

  // find view, relative to this filename
  // (FIXME: filename is set by ejs engine, other engines may need more help)
  var root = dirname(options.filename)
    , file = lookup(root, view, options)
    , key = file + ':string';
  if( !file )
    throw new Error('Could not find partial ' + view);

  // read view
  var source = options.cache
    ? cache[key] || (cache[key] = fs.readFileSync(file, 'utf8'))
    : fs.readFileSync(file, 'utf8');

  options.filename = file;

  // re-bind partial for relative partial paths
  options.partial = partial.bind(options);

  // render partial
  function render(){
    if (object) {
      if ('string' == typeof name) {
        options[name] = object;
      } else if (name === global) {
        // wtf?
        // merge(options, object);
      }
    }
    // TODO Support other templates (but it's sync now...)
    var html = ejs.render(source, options);
    return html;
  }

  // Collection support
  if (collection) {
    var len = collection.length
      , buf = ''
      , keys
      , prop
      , val
      , i;

    if ('number' == typeof len || Array.isArray(collection)) {
      options.collectionLength = len;
      for (i = 0; i < len; ++i) {
        val = collection[i];
        options.firstInCollection = i === 0;
        options.indexInCollection = i;
        options.lastInCollection = i === len - 1;
        object = val;
        buf += render();
      }
    } else {
      keys = Object.keys(collection);
      len = keys.length;
      options.collectionLength = len;
      options.collectionKeys = keys;
      for (i = 0; i < len; ++i) {
        prop = keys[i];
        val = collection[prop];
        options.keyInCollection = prop;
        options.firstInCollection = i === 0;
        options.indexInCollection = i;
        options.lastInCollection = i === len - 1;
        object = val;
        buf += render();
      }
    }

    return buf;
  } else {
    return render();
  }
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
