---
layout: post
title: React in Android WebViews
---

Many developers use the web to create UIs that can be reused across platforms. Although there are disadvantages to this (it is more difficult to create high-performance apps on all devices), it can be a good way to iterate quickly on multiple platforms.

[React](http://facebook.github.io/react/) is a javascript framework that enables building large, fast web apps. This post will describe how to use React in an Android WebView.

### Some javascript files

First you'll need some javascript files. You could reference these scripts by cdn url, but for mobile, where every byte over the wire is a burden, having the scripts on the device makes a big difference. Android's `assets/` folder is a reasonable place for these.

- Get the [latest version](http://facebook.github.io/react/downloads.html#individual-downloads) of React.

Older versions of Android, such as 2.3, have older versions of webkit powering the WebViews. To support these, we use [polyfills](http://facebook.github.io/react/docs/working-with-the-browser.html#browser-support-and-polyfills). We'll need

- es5-shim and es5-sham from [kriskowal's es5-shim](https://github.com/es-shims/es5-shim).

The header of you index.html (also in `assets/`) might look like this:

```html
<script src='es5-shim.js' type='text/javascript'></script>
<script src='es5-sham.js' type='text/javascript'></script>
<script src='react-0.11.1.js' type='text/javascript'></script>
```

Note that es5-shim must come *before* es5-sham.

### Setting up the WebView

To set up the WebView in Android, you'll then have to enable javascript by calling `setJavaScriptEnabled`. For newer Android versions, you may also need `setAllowFileAccessFromFileURLs` and `setAllowUniversalAccessFromFileURLs`, to [allow access](http://developer.android.com/reference/android/webkit/WebSettings.html#setAllowFileAccessFromFileURLs(boolean)) of your other assets.

Your Activity's onCreate might look like this:

```java
WebView webView = new WebView(this);
setContentView(webView);
WebSettings webSettings = webView.getSettings();
webSettings.setJavaScriptEnabled(true);
webView.loadUrl("file:///android_asset/index.html");
```

### Debugging

In newer versions of Android, you can [debug WebViews](https://developer.chrome.com/devtools/docs/remote-debugging#configure-webview) directly. For older versions, [jsconsole](http://jsconsole.com/) is a useful remote debugging tool that allows console access.

### That's it!

Just include a [React view](http://facebook.github.io/react/docs/getting-started.html) in your index.html.
