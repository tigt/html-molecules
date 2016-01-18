```html
<nav>
  <h2 class="visuallyhidden">Breadcrumbs</h2>

  <ol role="directory">
    <li typeof="v:Breadcrumb">
      <a href="/"
         rel="index v:url"
         property="v:title">Home</a>
    </li>

    <li typeof="v:Breadcrumb">
      <a href="../.."
         rel="v:url"
         property="v:title">Double parent</a>
    </li>

    <li typeof="v:Breadcrumb">
      <a href=".."
         rel="v:url"
         property="v:title">Parent</a>
    </li>

    <li typeof="v:Breadcrumb">
      <a href="./"
         rel="v:url"
         property="v:title">Current page</a> <!-- not a fan of including the `href` on here, since clicking a link to go to where you currently are isn't very user-friendly. If it's necessary, we'll have to do awful stuff with tabindex="-1" and pointer-events: none -->
    </li>
  </ol>
</nav>
```

```css
li:not(:last-child)::after {
  content: " ↪";
  alt: " then";
}
```

The RDFa cruft is so [Google results look nice](https://developers.google.com/structured-data/breadcrumbs).

RDfa syntax is shorter than `itemscope` and friends and has a guaranteed future (it's a W3C spec, and microdata can't even get a maintaining editor). In particular, it's nice dropping `prefix` because [the `v:` prefix is predefined](https://www.w3.org/2011/rdfa-context/rdfa-1.1). Google accepts both. I'm like to make it RDFa Lite (shorter and friendlier), but that'll require testing to see if Google accepts the old vocabulary and the new syntax together. The [W3C RDFa Validator](https://www.w3.org/2012/pyRdfa/Validator.html) will be helpful there.

I'm using [data-vocabulary.org's breadcrumbs](http://www.data-vocabulary.org/Breadcrumb/) because it's less verbose than [schema.org's](http://schema.org/BreadcrumbList) (inline `<meta>` tags? Really?), and Google still recognizes it. ([data-vocabulary.org was Google's self-standard](https://web.archive.org/web/20110902135518/http://www.google.com/support/webmasters/bin/answer.py?&answer=185417) before they teamed up for schema.org). If not, the longer and uglier version looks like this:

```html
<nav aria-label="Breadcrumbs">
  <ol role="directory"
      vocab="http://schema.org/" typeof="BreadcrumbList">
    <li property="itemListElement" typeof="ListItem">
      <a href="/"
         rel="index"
         property="item" typeof="WebPage">
        <span property="name">Home</span>

        <meta property="position" content="1">
      </a>
    </li>

    <li property="itemListElement" typeof="ListItem">
      <a href="../.."
         property="item" typeof="WebPage">
        <span property="name">Double parent</span>

        <meta property="position" content="2">
      </a>
    </li>

    <li property="itemListElement" typeof="ListItem">
      <a href=".."
         property="item" typeof="WebPage">
        <span property="name">Parent</span>

        <meta property="position" content="3">
      </a>
    </li>

    <li property="itemListElement" typeof="ListItem">
      <a property="item">You are here</a>

      <meta property="position" content="4">
    </li>
  </ol>
</nav>
```
