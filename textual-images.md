# Textual images

When is text an image? As far as the web is concerned, when you attempt to read it out loud.

## ASCII art

```html
<pre role="img"
     aria-label="A cat">
  |\_/|
 / @ @ \
( > º < )
 `>>x<<´
 /  O  \
</pre>
```

This sort of thing is especially bad with what I like to call "programmer documentation art":

```html
<pre role="img"
     aria-label="Your working directory should now contain the file index.html, robots.txt, and favicon.ico">
/working-directory/
                  |-index.html
                  |-robots.txt
                  \-favicon.ico
</pre>
```

Complex ASCII art, like, say, [the diagrams in RFCs](http://tools.ietf.org/html/draft-ash-alt-formats-02#section-4), deserve a bit more structure:

```html
<figure>
<pre role="img"
     aria-labelledby="figcaption-1"
     aria-describedby="figdescription-1">
+--------------+
|              |        Server Acquisition
|  AC issuer   +＜---------------------------+
|              |                            |
+--+-----------+                            |
   ^                                        |
   | Client                                 |
   | Acquisition                            |
   v                                        v
+--+-----------+                         +--+------------+
|              |       AC "push"         |               |
|   Client     +＜------------------------|    Server     |
|              | (part of app. protocol) |               |
+--+-----------+                         +--+------------+
   ^                                        ^
   | Client                                 | Server
   | Lookup        +--------------+         | Lookup
   |               |              |         |
   +-------------->+  Repository  +＜--------+
                   |              |
                   +--------------+
</pre>

  <figcaption id="figcaption-1">Figure 1: AC Exchanges</figcaption>
</figure>

<p id="figdescription-1">An abstract view of the exchanges that may involve ACs: Client Acquisition, Client Lookup, Server Acquisition, Server Lookup, and AC "push" (part of the application protocol).</p>
```

But frankly, once you get to this point, you should really be using SVG instead, which has [some upcoming complex-graphics roles](https://rawgit.com/AmeliaBR/aria/aria-graphics/aria/graphics.html#roles) that will do nicely.

## Text emoticons

```html
<span role="img"
      aria-label="Embarrassed face">(^_^;)</span>
```

It's not likely you'll get users to mark up their smilies like this, but when publishing existing content, a little extra effort is all you need for accessibility.

## Unicode characters

## Emoji

Emoji are going to be interoperable one day, but right now using raw emoji characters on the web is inadvisable:

* Assistive technology hasn't caught up yet, so they tend to

```html
<span role="img" aria-label="">😃</span>
```

If you're using an emoji-to-image replacement for deeper support (missing fonts, no Unicode multibyte support, proxy browsers, browser bugs), like [Twitter and WordPress do](https://blog.twitter.com/2014/open-sourcing-twitter-emoji-for-everyone), you're halfway there:

```html
<img src="./1f389.png"
     draggable="false"
     alt="🎉"
     title="Party popper"
     aria-label="Emoji: Party popper">
```

Twitter has done an excellent job with their emoji-replacement markup:

* Putting the real emoji character in `alt` makes copy and paste work like it should.
* `draggable="false"` makes selections act like it was just text.
* The `title` attribute is for when you have no clue what that emoji is supposed to be. It even works on mobile nowadays; you can touch-and-hold images with a `title` to read it at the top of the context menu on both iOS and Android nowadays.
* `aria-label` compensates for assistive technology largely not knowing how to pronounce most emoji yet.

And best of all, [their emoji-replacement library is open-source](https://github.com/twitter/twemoji)! Too bad [it doesn't add the `title` and `aria-label` yet](https://github.com/twitter/twemoji/issues/41). I would recommend avoiding using this library on the client-side, as a result.

If you really wanted to go the extra mile, I would attach a SVG version of the emoji using `srcset` (with the older, better-supported `x` notation), so newer browsers can display a more scalable version.

```html
<img src="./1f389.png"
     srcset="./1f389.svg"
     draggable="false"
     alt="🎉"
     title="Party popper"
     aria-label="Emoji: Party popper">
```
