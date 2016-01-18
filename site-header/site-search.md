```html
<form role="search"
      action="{site search URL goes here}"
      property="potentialAction" typeof="SearchAction">
  <input type="search"
         name="q"
         placeholder="Search..."
         accesskey="4"
         autosave="reyrey.com"
         maxlength="{Whatever the maximum number of characters the search engines supports is; if none, omit this attribute entirely}"
         minlength="{Like above, but the reverse}"
         required
         pattern=".*\S.*"
         title="Enter something to search for"
         property="query-input"/>
  <meta property="target" content="https://example.com/search?q={q}">

  <button type="submit">Go</button>
</form>
```

The `pattern` attribute requires the user to enter something other than white space, to prevent spurious requests for, say, ` `.

`accesskey="4"` is a de-facto standard for activating site search.

If search suggestions should be used, we'll need a `<datalist>` and [the `incremental` attribute](https://developers.google.com/structured-data/slsb-overview).

As a bonus, this information can be used to automatically generate an OpenSearch plugin, which is a nice bonus for Chrome & Firefox users. It looks something like this:

```html
<link rel="search"
      type="application/opensearchdescription+xml"
      href="/osd.xml"
      title="{{Site Name}} search"/>
```

```xml
<OpenSearchDescription xmlns="http://a9.com/-/spec/opensearch/1.1/"
                       xmlns:moz="http://www.mozilla.org/2006/browser/search/">
  <ShortName>{16 or fewer characters of plain text}</ShortName>

  <Description>{{1024 or fewer characters of plain text}}</Description>

  <InputEncoding>UTF-8</InputEncoding>

  <Image width="16" height="16" type="image/x-icon">{{Icon URL, .ICO file}}</Image>

  <Image width="64" height="64" type="image/png">{{Larger icon URL, .PNG file}}</Image>

  <Url type="text/html" template="{{the above <form>'s action attribute}}">
    <Param name="q" value="{searchTerms}"/>
    <Param name="s" value="opensearch"/>
    <!-- if the engine supports additional parameters, we can use some of the ones here: http://www.opensearch.org/Specifications/OpenSearch/1.1/Draft_3#OpenSearch_1.1_parameters -->
  </Url>

  <Url type="application/x-suggestions+json" template="https://example.com/suggestions?q={searchTerms}"/> <!-- if your engine has autosuggest -->

  <Url type="application/opensearchdescription+xml"
     rel="self"
     template="{{this osd.xml file's location}}"/>

  <Query role="example" searchTerms="car"/>

  <moz:SearchForm>{{main site's search page}}</moz:SearchForm>
</OpenSearchDescription>
```
