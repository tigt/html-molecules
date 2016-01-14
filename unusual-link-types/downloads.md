```html
<a href="${URL}"
   download="${suggested filename}"
   target="_blank"
   type="${file's media type}">Download ${thing}…</a>
```

If `${URL}` is a HTTP(S) file, make sure to set the `Content-Disposition` header as well.

Unlike normally, `target="_blank"` is good UX here.

The `download` attribute does not work across origins.
