```html
<a href="javascript:..."
   title="See below for detailed installation instructions"
   type="application/javascript">Suggested bookmark name</a>

<h2>How to use this bookmarklet</h2>

If you are on iOS...

<figure>
  <figcaption>Bookmarklet source code</figcaption>

  <pre><code>
    ${prettified bookmarklet code}
  </code></pre>
</figure>
```

```js
javascript:void(sUrl=prompt('view source of:',top.location.href));if(sUrl){void(agt=navigator.userAgent.toLowerCase());if(agt.indexOf('mac')!=-1 && agt.indexOf('msie')!=-1){sPrefix=''}else{sPrefix='view-source:'}void(location.href=sPrefix+sUrl)}
```

`title` is also shown when long-pressing links on mobile nowadays, so you can use it alert mobile users that the bookmarking process for the bookmarklet is... unintuitive.

## Coding bookmarklets

http://benalman.com/projects/run-jquery-code-bookmarklet/

Make sure any pulled-in resources that your bookmarklet calls are accessible over HTTPS, otherwise it won't run on increasingly large swathes of the web.

If your bookmarklets uses any variables, always use an immediately-invoked function expression (IIFE), otherwise you'll doubtless run into variable collisions.

```js
(function bookmarklet() {
  /* Your code goes here */
}());
```

Naming the IIFE isn't necessary, but I find it helpful when debugging, especially directing users to copy something out of the console in devtools.

The grapevine says bookmarklets should be shorter than 2000 characters. If you need more than that, load an external script:

```js
var s = document.createElement('script');
s.src = 'https://mysite.com/script.js';
document.body.appendChild(s);
```

But don't load libraries twice! https://www.smashingmagazine.com/2010/05/make-your-own-bookmarklets-with-jquery/

Once you're all done, for maximum compatibility, you'll need to escape and URL-encode your snippet. Try [Mr. Cole's tool](https://github.com/mrcoles/bookmarklet).

Be aware that users like to do things such as double-click bookmarklets.

Is it possible to associate an icon with a bookmarklet, by using the default "navigate to returned string" behavior of `javascript:`?
