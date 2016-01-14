## Email address basic bot-prevention

You hear that "this won't stop the determined ones." Perhaps. But we ought to make doing such terrible deeds as difficult as possible, yes? Given that we're working within layers upon layers of standards, we should be able to find enough wiggle room to craft technically-valid, clickable URLs that no naive parser would have a hope of understanding.

* Unicode code points
* Unicode bidirectional algorithm
* HTML text parsing
* HTML attribute parsing
* HTML character references
* Relative and base URLs
* DNS trickery
* URL anatomy trickery
* URL percent-encoding
* `mailto:` scheme trickery

(Note to self: [the `.invalid` domain should probably be used for these examples or something](https://en.wikipedia.org/wiki/.invalid))

```html
<a href="mailto:me@example.com">me@example.com</a>
```

### `<base>`?

If this worked, it would be hilarious:

```html
<base href="mailto:" />
```

It's valid, so.

This would be most useful if this worked:

```html
<base href="mailto:me@" />

<a href="example.com"></a>
```

But I can't imagine that does. So this will have to do:

```html
<base href="mailto:" />

<a href="?to=me@example.com"></a>
```

### The bidrectional algorithm

This only works if you're not providing a `mailto:` link.

Does `<ruby>` allow this sort of thing, too? Even if not, there should be an infinite number of HTML hacks if you're forcing visitors to copy and paste.

Which might not be too bad, if the confusion surrounding `mailto:` links is to be believed.

### Attribute parsing rules

HTML is so lenient, let's put that to work! If the baddies aren't using a full-fat HTML parser like an actual browser, this should be much more difficult.

#### Extra spaces

```html
<a href  =  "   mailto: tigt     @   mortropolis .  com "></a>
```

The HTML5 spec defines a lot of whitespace characters for parsing attribute values:

#### Attribute delimiters

Either unquoted or single-quoted.

Trailing slashes at the end of unquoted attributes could also help a little.

### Additional URL garbage

Some of these strategies may work great with "catch-all" email addresses for a particular domain.

Have you ever seen the railroad diagram for a proper regular expression to validate an email address? There's a LOT of stuff you can add to a `mailto:` URL to make it look funny without ruining its functionality!

* Comments: `local-part(comment)@example.com`, `(comment)local-part@example.com`, also work inside the domain...s
* Quoted strings: `abc."defghi".xyz@example.com`, `"abcdefghixyz"@example.com`. Beware, may not work with all mailboxes.
* If we can fit extra `@` symbols and spaces inside these constructs, that will REALLY foul up crawlers
* Backslash-escaping strings apparently works? Can add unbalanced quotes, double slashes, or escaped line-breaks to an address
* Empty password section of the auth part of URLs
* Group addressing: `foo:a@b.com,b@b.com,etc@and-so-on.com`? Might be out of scope
* Domain name normalization

#### Unnecessary Punycode escaping

`xn--example-.xn--com-` maps to `example.com`

#### Mail to nonexistent address and include real address

Either the canonical way:

```html
<a href="not-real@fake.com,me@example.com">Email me at me@example.com</a>
```

Or with query-string-supplied headers:

```html
<a href="not-real@fake.com?to=me@example.com"></a>

or

<a href="not-real@fake.com?cc=me@example.com"></a>
```

You can even leave off the first address, like this: `mailto:?parameters`.

#### Things to try

* Trailing slash (`me@example.com/`)
* Trailing comma (since `mailto:` accepts multiple email addresses)
* Case-insensitive domain name (`UPPERCASE` and `mIxEd CaSe` are valid)
* Bogus port number (or the real one, usually `:25`, but can be changed!)
* Gmail lets you specify bogus `.` that it ignores (even consecutive ones, which are invalid), and disposable `+label`s, which can help with auto-deleting spam
* Unicode characters in the domain (might not work everywhere)

### URL Encoding

Notably, I think HTML is lenient enough that you can have invalid lower-case percent-encoding, but I'd need to double-check.

The `local-part` (bits before the `@`) can be URL-encoded.

### Character entities

Don't forget: the white space and `mailto:` can be entitified too!

And since it's a URL, you can even percent-encode those entities, or vice-versa!

#### Named entities

@ is `&commat;`
: is `&colon;`
; is `&semi;`
. is `&period;`
+ is `&plus;`
/ is `&sol;`
\ is `&bsol;`
* has a bunch, if it can be used in an address (`&sstarf;`, `&Star;`, `&ast;`, etc.)
~ is `&Tilde;`
_ is `&UnderBar;`
| is `&verbar;`, `&vert;`, `&VerticalLine;`
␉ is `&Tab;`
␊ is `&NewLine;`
␌ is ` `
␍ is ` `
& can be encoded as `&amp;`, `&amp`, `&AMP;`, or `&AMP`
, is `&comma;`
- is `&hyphen;`
Can (), {}, and [] be used for anything? They all have named entities
' is `&quot;` in XML and in HTML5: `&QUOT;`, `&QUOT`, and `&quot`
`&nbsp;`? (aliases: `&NoBreak;` and `&NonBreakingSpace;`)
<>, of course

Don't forget that ␍␊ can be assembled a few different ways.

These may not work in older browsers; more testing needed.

#### Decimal

`&#116;&#105;&#103;&#116;&#64;&#109;&#111;&#114;&#116;&#114;&#111;&#112;&#111;&#108;&#105;&#115;&#46;&#99;&#111;&#109;`

#### Unicode

`&#x74;&#x69;&#x67;&#x74;&#x40;&#x6D;&#x6F;&#x72;&#x74;&#x72;&#x6F;&#x70;&#x6F;&#x6C;&#x69;&#x73;&#x2E;&#x63;&#x6F;&#x6D;`

In HTML, the leading `x` can be either case, which provides another fine source of entropy. Be careful, though; in XML, it must be lowercase.

For both kinds of numeric URLs, leading zeroes are also acceptable. (How much?)

#### Ultra-wacky

As many named entities as can be had, as many grandfathered-in invalid cases as can be had (e.g. `&AMP`), mixture of decimal and hexadecimal, alternating-case `x` in hexadecimal, varying leading zeroes for numeric references

### Garbage characters

* Left-to-right mark: `&#8206;`/`&lrm;`
* Right-to-left mark: `&#8207;`/`&rlm;`
* Various white space kinds; may not work!
* Byte-Order Mark

#### Invalid Unicode characters (risky!)

`U+0000` NULL
`U+FFFD` REPLACEMENT CHARACTER (also works with character reference parsing algorithm)
Failure to produce valid character combination

### Inline SVG

This obviously only works in browsers that can handle inline SVG. Not of tremendous use as a result.

#### XLink

`<text><a xlink:href="mailto:me@example.com">Email me at me@example.com</a></text>`

#### CDATA sections

Is it possible to use `<!CDATA[` here? Should be for the text inside the elements, anyway.

#### `xml:base`

Only if relative `mailto:` URLs work, somehow.

### Wackier methods

* Space with table
* Break up with images
* JavaScript-only
* Roll your own email entity in an XML doctype (guaranteed to work almost nowhere!)
* Use inline SVG
* CSS-only

### Putting it all together

This ought to work for mostly everything:

1. Obfuscated protocol in `<base>` (Ultra-wacky character reference encoding, whitespace-padded attribute value)
2. Reversed characters in bidirectional algorithm
3. Dropped junk characters in HTML parser (BOM, make the `<a>` difficult to parse, etc.)
4. Whitespace-padded attribute value
5. Bogus `(comment)`s attached to address with confusing characters inside, possible additions depending on known email provider (Gmail is fine with additional `.`, for example)
6. Mixed-case, unnecessary punycode domain name
7. All of that stuffed into `mailto:`'s `?to=` field, after a couple of bogus empty addresses
8. Lower/mixed-case and unnecessary percent-encoding
9. Ultra-wacky character reference encoding

By the way, only use with UTF-8. Nobody can predict what might happen if you don't.

Simulated result:

```html
<base href  =
 "
 mailto:
    "
 >
<a href =  "
    &#063;to=&quot;me@xN--eXAmpLE-.Xn--cOm&quot;
"></a>
```

## Form basic bot-prevention with honeypots:

<form>
  <input name="honeypot" type="hidden" />

  <input name="honeypot" style="display: none"
                         hidden
                         placeholder="Don't fill me out!" />

  <input name="honeypot" style="position: absolute;
                                opacity: 0;
                                pointer-events: none;
                                clip: rect(0 0 0 0)"
                                tabindex="-1"
                                placeholder="Don't fill me out!" />
</form>

For best results, name it something other than "honeypot"; like, say, "captcha".
