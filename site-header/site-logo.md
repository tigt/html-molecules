```html
<html lang="en-us" vocab="http://schema.org/" typeof="WebSite">

<!-- ... -->

<header>
  <h1>
    <a href="/"
       rel="index"
       property="url">
      <svg>
        <title property="name">Reynolds &amp; Reynolds</title>

        <image typeof="Organization" property="logo" xlink:href=""/>
      </svg>
    </a>
  </h1>
</header>
```

Two `<h1>`s seem weird at first, but it makes more sense the longer I think about it, and [screen-reader users prefer it](http://webaim.org/projects/screenreadersurvey3/#headings).

https://developers.google.com/structured-data/site-name

https://developers.google.com/structured-data/customize/logos
