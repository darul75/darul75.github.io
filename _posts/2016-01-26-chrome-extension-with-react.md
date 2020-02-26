---
layout: post
title: Chrome extension with React
---

Previous blog [post](http://www.darul.io/post/2016-01-14_canvas-image-and-data-uri) was about manipulating images with Javascript API and here is a recap of my notes after creating a new Chrome extension app available [here](https://chrome.google.com/webstore/detail/dog-it/bcceanhkdnodhlglnggnepecgoemgmmg/related).

## Goal

Create a chrome extension to fetch and download all displayed (or not) images from current web page.

## Stack

`100% Javascript` with `ES6` sugar syntax and `React` library.

My first try of creating a chrome prototype app few years ago was done with a popular framework you may have known called AngularJS is still [available](https://chrome.google.com/webstore/detail/bootstrapy/kjgakbmoeokpdkpjefebfnhmclfiiofg) for free :)

Then I had to choose the right tool to bundle appplication. Gulp, Grunt, Browserify, Webpack, Systemjs... javascript fatigue ? [@Vjeux](https://twitter.com/Vjeux) has already pointed out that is quite difficult today to start a new JS project [quickly](http://blog.vjeux.com/2015/javascript/challenge-best-javascript-setup-for-quick-prototyping.html)

I had already used webpack, grunt or gulp and a try with browserify which was really nice so it was my end choice combined with awesome [watchify](https://github.com/substack/watchify) tool.

## Development

Before to start developing your own chrome extension a nice overview is given [here](https://developer.chrome.com/extensions/overview) and [here](https://developer.chrome.com/extensions/getstarted).

Then you might fork any of these provided samples from this [Google Github Repository](https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples). Off course you will have to clean it, rename, remove files...boring I know :)

Ok now you are ready. 

Keep in mind that React documentation is always [useful](https://facebook.github.io/react/docs/getting-started.html) and may changes with version updates.

## Browserify and watchify

Browserify bundles your javascript files while [watchify](https://github.com/substack/watchify) triggers this processus to be run in background when updates occured in your source files.

Installation is an npm standard:

```bash
npm i -g browserify watchify
```

Basic watchify command could be 

```bash
watchify main.js -o static/bundle.js
```

Mine

```bash
watchify --debug -t [ babelify --presets [ es2015 react ] --plugins 
[transform-object-rest-spread] ] src/app/app.js -o extension/bundle.js
```

This way, you will got all [Babel](https://babeljs.io/) transpiler features available depending on your specific ES6 sugar needs combined with React library. See my [package.json](https://github.com/darul75/fetchme/blob/master/package.json) for dependencies check.

**Common errors**

Maybe some of you will one day get this warning.

```bash
Error: Parsing file app.js: 'import' and 'export' may appear only 
with 'sourceType: module'
```

Don't panic, that was just a missing parameter with `es2015` babel preset :

```bash
> watchify --debug -t [ babelify --presets [ react ] ] ...
```

to
```bash
> watchify --debug -t [ babelify --presets [ es2015 react ] ] ...
```

Small bash script [here](https://github.com/darul75/fetchme/blob/master/build.sh)

## Content script

Mainly what you need to keep in mind is that a chrome extension can interact with your current page, read its content, update it...by injection of some "content javascript file" as described [here](https://developer.chrome.com/extensions/overview#contentScripts)

"Extensions can interact with web pages or servers using `content scripts` or cross-origin XMLHttpRequests. Extensions can also interact programmatically with browser features such as bookmarks and tabs."

![Console](/images/posts/chrome-content-script.png)

## Messaging system

Event driven chrome extension API is quite simple and permit bidirectional communication between *extension* and *contentscript* side. 

**Send** 

```javascript
sendMessage(tabId, request, callback)
```

[details](https://developer.chrome.com/extensions/runtime#method-sendMessage)

**Receive**

```javascript
onMessage.addListener(callback);
```
[details](https://developer.chrome.com/extensions/runtime#event-onMessage)

See below for a listener callback example.

It is very intuitive and you can query to get current chrome tab or others by instance.

Sent messages use JSON format, so everything is serialized this way, my choice was to deeply use [base64 data encoding system](http://www.darul.io/post/2016-01-14_canvas-image-and-data-uri#data-uri-format-specification). A process of deserialization is done each time a message is captured by an event listener on content script side of extension side (parseJSON ?), heavy task ?....

Be careful with messaging system and *synchronous* or *asynchronous* task.

In case of asynchronous task, just remember to *return true* in your event listener handler function.

**Synchronous receiver**
```javascript
(request, sender, sendResponse) => {
  // doSomething synchronous
  doSync();
  // sendResponse is optional but useful to send back to origin
  sendResponse({data: 'this was synchronous'});
  // return false by default
}
```

**ASynchronous receiver**
```javascript
(request, sender, sendResponse) => {
  // doSomething synchronous, Promise, Async, Callback... 
  doASync( 
    (data) => {
      sendResponse({data: 'this was asynchronous'});
    }
  );    
  return true; // MANDATORY
}
```

[Chrome API](https://developer.chrome.com/extensions/runtime#event-onMessage)
[More](http://stackoverflow.com/questions/20077487/chrome-extension-message-passing-response-not-sent)

## Conclusion

Dealing with chrome API is simple but debugging it ( both *extension/contentscript* side) into browser, dev consoles, is sometimes annoying because you need to activate extension to get chrome context, reload you extension manually often from here chrome://extensions/

I was thinking about to mock chrome API in order to fully concentrate on my code, but it looks like guys have already done it, I will give a try later

Code is available on this [repository](https://github.com/darul75/fetchme)

https://github.com/acvetkov/sinon-chrome
https://github.com/sethmcl/chrome-mock

![Just for fun](/images/posts/frog.jpg)

----------

Tags: *Angular* *Chrome* *Javascript* *React*
