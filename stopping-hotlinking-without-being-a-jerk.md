# Stopping Hotlinking Without Being a Jerk

Instead of just forbidding image access without the proper Referrer, instead serve an HTML page with the image embedded, and a link to the site and context where that image appeared.

The first inclination to work with this is to check the `Referer` header, but I'm pretty sure some browsers (like Tor) strip that out, and with HTTPS-everywhere happening I dunno how that header will make the leap.

The other idea is keying off of the `Accept` header to check if it's [expecting an image file](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation#Values_for_an_image). From what I can tell, [it's a bit of a mess](http://www.newmediacampaigns.com/blog/browser-rest-http-accept-headers), but it should still be usable. These two links are definitely out-of-date; Chrome certainly sends `image/webp` nowadays.

Using both headers for an "intent scent" should be okay. The presence or absence of other headers may help.

Kind of like Tumblr's "image" pages (http://staff.tumblr.com/image/131973825945)

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>{{image title}} | {{site name}}</title>

  <meta name="description" content="{{image title|desc}}">
  <meta name="theme-color" content="{{site theme color}}">
  <!-- icons and nonsense -->
  <style>
    img {
      max-width: 100%;
      height: auto;
    }

    img.fullsize {
      max-width: none;
    }
  </style>

  <link rel="canonical" href="{{either this URL, or the embedded context...}}">
</head>
<body>
  <a href="{{embedded context}}">← Image from {{embedded Title}}</a>

  <p>{{image title}} | {{image filename}}</p>

  <img src="{{the image in question}}" alt="{{image title}}">
</body>
</html>
```
