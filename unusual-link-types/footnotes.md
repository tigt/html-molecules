```html
<sup role="note">
  <a href="#note-1"
     id="ref-1"
     title="Footnote 1">[1]</a>
</sup>

<!-- -->

<section>
  <h3>Footnotes</h3>

  <ol>
    <li id="note-1">
      <a href="#ref-1"
         title="">↩</a>
    </li>
  </ol>
</section>
```

```css
sup[role=note] { }

li[id^=note-]:target { }
```
