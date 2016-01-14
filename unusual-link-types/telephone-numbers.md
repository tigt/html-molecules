```html
<a href="tel:+1(555)555-5555.55">+1 (555) 555-5555 x55</a>
```

https://tools.ietf.org/html/rfc2806

I think the `;isub=` parameter is for extensions? Hm.

```css
a[href^=tel:] {
  speak: digits;
}
```

Available separator characters are `-`, `.`, `(`, and `)`. The above usage is primarily for US numbers; other countries have different conventions.

If you're stuck in the stone age and are providing a fax number, it's similar, but with `fax:`.
