# api documentation for  [node-wit (v4.2.0)](https://github.com/wit-ai/node-wit#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-node-wit.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-node-wit) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-node-wit.svg)](https://travis-ci.org/npmdoc/node-npmdoc-node-wit)
#### Wit.ai Node.js SDK

[![NPM](https://nodei.co/npm/node-wit.png?downloads=true)](https://www.npmjs.com/package/node-wit)

[![apidoc](https://npmdoc.github.io/node-npmdoc-node-wit/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-node-wit_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-node-wit/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-node-wit/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-node-wit/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "The Wit Team",
        "email": "help@wit.ai"
    },
    "bugs": {
        "url": "https://github.com/wit-ai/node-wit/issues"
    },
    "dependencies": {
        "isomorphic-fetch": "^2.2.1",
        "uuid": "^3.0.0"
    },
    "description": "Wit.ai Node.js SDK",
    "devDependencies": {
        "babel-cli": "^6.16.0",
        "babel-preset-es2015": "^6.16.0",
        "babel-preset-stage-0": "^6.16.0",
        "chai": "^3.5.0",
        "mocha": "^3.1.2",
        "sinon": "^1.17.6"
    },
    "directories": {},
    "dist": {
        "shasum": "e919d2f982f0331ae214d55f92bd3e4f33d5b986",
        "tarball": "https://registry.npmjs.org/node-wit/-/node-wit-4.2.0.tgz"
    },
    "engines": {
        "node": ">=4.0.0"
    },
    "homepage": "https://github.com/wit-ai/node-wit#readme",
    "keywords": [
        "wit",
        "wit.ai",
        "bot",
        "botengine",
        "bots",
        "nlp",
        "automation"
    ],
    "main": "index.js",
    "maintainers": [
        {
            "name": "blandinw",
            "email": "willy@wit.ai"
        },
        {
            "name": "catacola",
            "email": "akesich@gmail.com"
        },
        {
            "name": "martinraison",
            "email": "martinraison@gmail.com"
        },
        {
            "name": "oliviervaussy",
            "email": "oliv@wit.ai"
        },
        {
            "name": "patapizza",
            "email": "julien@wit.ai"
        },
        {
            "name": "stopachka",
            "email": "stepan.p@gmail.com"
        }
    ],
    "name": "node-wit",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/wit-ai/node-wit.git"
    },
    "scripts": {
        "test": "mocha ./tests/lib.js"
    },
    "version": "4.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module node-wit](#apidoc.module.node-wit)
1.  [function <span class="apidocSignatureSpan">node-wit.</span>Wit (opts)](#apidoc.element.node-wit.Wit)
1.  [function <span class="apidocSignatureSpan">node-wit.</span>interactive (wit, initContext, maxSteps)](#apidoc.element.node-wit.interactive)
1.  object <span class="apidocSignatureSpan">node-wit.</span>log

#### [module node-wit.log](#apidoc.module.node-wit.log)
1.  [function <span class="apidocSignatureSpan">node-wit.log.</span>Logger (lvl)](#apidoc.element.node-wit.log.Logger)
1.  string <span class="apidocSignatureSpan">node-wit.log.</span>DEBUG
1.  string <span class="apidocSignatureSpan">node-wit.log.</span>ERROR
1.  string <span class="apidocSignatureSpan">node-wit.log.</span>INFO
1.  string <span class="apidocSignatureSpan">node-wit.log.</span>WARN



# <a name="apidoc.module.node-wit"></a>[module node-wit](#apidoc.module.node-wit)

#### <a name="apidoc.element.node-wit.Wit"></a>[function <span class="apidocSignatureSpan">node-wit.</span>Wit (opts)](#apidoc.element.node-wit.Wit)
- description and source-code
```javascript
function Wit(opts) {
  var _this = this;

  if (!(this instanceof Wit)) {
    return new Wit(opts);
  }

  var _config = this.config = Object.freeze(validate(opts)),
      accessToken = _config.accessToken,
      apiVersion = _config.apiVersion,
      actions = _config.actions,
      headers = _config.headers,
      logger = _config.logger,
      witURL = _config.witURL;

  this._sessions = {};

  this.message = function (message, context) {
    var qs = 'q=' + encodeURIComponent(message);
    if (context) {
      qs += '&context=' + encodeURIComponent(JSON.stringify(context));
    }
    var method = 'GET';
    var fullURL = witURL + '/message?' + qs;
    var handler = makeWitResponseHandler(logger, 'message');
    logger.debug(method, fullURL);
    return fetch(fullURL, {
      method: method,
      headers: headers
    }).then(function (response) {
      return Promise.all([response.json(), response.status]);
    }).then(handler);
  };

  this.converse = function (sessionId, message, context, reset) {
    var qs = 'session_id=' + encodeURIComponent(sessionId);
    if (message) {
      qs += '&q=' + encodeURIComponent(message);
    }
    if (reset) {
      qs += '&reset=true';
    }
    var method = 'POST';
    var fullURL = witURL + '/converse?' + qs;
    var handler = makeWitResponseHandler(logger, 'converse');
    logger.debug(method, fullURL);
    return fetch(fullURL, {
      method: method,
      headers: headers,
      body: JSON.stringify(context)
    }).then(function (response) {
      return Promise.all([response.json(), response.status]);
    }).then(handler);
  };

  var continueRunActions = function continueRunActions(sessionId, currentRequest, message, prevContext, i) {
    return function (json) {
      if (i < 0) {
        logger.warn('Max steps reached, stopping.');
        return prevContext;
      }
      if (currentRequest !== _this._sessions[sessionId]) {
        return prevContext;
      }
      if (!json.type) {
        throw new Error('Couldn\'t find type in Wit response');
      }

      logger.debug('Context: ' + JSON.stringify(prevContext));
      logger.debug('Response type: ' + json.type);

      // backwards-compatibility with API version 20160516
      if (json.type === 'merge') {
        json.type = 'action';
        json.action = 'merge';
      }

      if (json.type === 'error') {
        throw new Error('Oops, I don\'t know what to do.');
      }

      if (json.type === 'stop') {
        return prevContext;
      }

      var request = {
        sessionId: sessionId,
        context: clone(prevContext),
        text: message,
        entities: json.entities
      };
      if (json.type === 'msg') {
        var response = {
          text: json.msg,
          quickreplies: json.quickreplies
        };
        return runAction(actions, 'send', request, response).then(function (ctx) {
          if (ctx) {
            throw new Error('Cannot update context after \'send\' action');
          }
          if (currentRequest !== _this._sessions[sessionId]) {
            return ctx;
          }
          return _this.converse(sessionId, null, prevContext).then(continueRunActions(sessionId, currentRequest, message, prevContext
, i - 1));
        });
      } else if (json.type === 'action') {
        return runAction(actions, json.action, request).then(function (ctx) {
          var nextContext = ctx || {};
          if (currentRequest !== _this._sessions[sessionId]) {
            return nextContext;
          }
          return _this.converse(sessionId, null, nextContext).then(continueRunActions(sessionId, currentRequest, message, nextContext
, i - 1));
        });
      } else {
        logger.debug('unknown response type ' + json.type);
        throw new Error('unknown response type ' + json.type);
      }
    };
  };

  this.runActions = function (sessionId, message, context, maxSteps) {
    var _this2 = this;

    if (!actions) throwMustHaveActions();
    var steps = maxSteps ? maxSteps : DEFAULT_MAX_STEPS;
    // Figuring out whether we need to reset the last turn.
    // Each new call inc ...
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.node-wit.interactive"></a>[function <span class="apidocSignatureSpan">node-wit.</span>interactive (wit, initContext, maxSteps)](#apidoc.element.node-wit.interactive)
- description and source-code
```javascript
interactive = function (wit, initContext, maxSteps) {
  var context = (typeof initContext === 'undefined' ? 'undefined' : _typeof(initContext)) === 'object' ? initContext : {};
  var sessionId = uuid.v1();

  var steps = maxSteps ? maxSteps : DEFAULT_MAX_STEPS;
  var rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
  });
  rl.setPrompt('> ');
  var prompt = function prompt() {
    rl.prompt();
    rl.write(null, { ctrl: true, name: 'e' });
  };
  prompt();
  rl.on('line', function (line) {
    line = line.trim();
    if (!line) {
      return prompt();
    }
    wit.runActions(sessionId, line, context, steps).then(function (ctx) {
      context = ctx;
      prompt();
    }).catch(function (err) {
      return console.error(err);
    });
  });
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.node-wit.log"></a>[module node-wit.log](#apidoc.module.node-wit.log)

#### <a name="apidoc.element.node-wit.log.Logger"></a>[function <span class="apidocSignatureSpan">node-wit.log.</span>Logger (lvl)](#apidoc.element.node-wit.log.Logger)
- description and source-code
```javascript
function Logger(lvl) {
  var _this = this;

  this.level = lvl || INFO;

  levels.forEach(function (x) {
    var should = levels.indexOf(x) >= levels.indexOf(lvl);
    _this[x] = should ? funcs[x] : noop;
  });
}
```
- example usage
```shell
...
  opts.witURL = opts.witURL || DEFAULT_WIT_URL;
  opts.apiVersion = opts.apiVersion || DEFAULT_API_VERSION;
  opts.headers = opts.headers || {
    'Authorization': 'Bearer ' + opts.accessToken,
    'Accept': 'application/vnd.wit.' + opts.apiVersion + '+json',
    'Content-Type': 'application/json'
  };
  opts.logger = opts.logger || new log.Logger(log.INFO);
  if (opts.actions) {
    opts.actions = validateActions(opts.logger, opts.actions);
  }

  return opts;
};
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
