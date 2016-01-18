```html
<details>
  <summary><label for="toggle">Summary label</label></summary>
  <input type="checkbox"
         id="toggle"
         checked/>

  <!-- whatever you want goes here -->
</details>

<style>
  #toggle:checked ~ * {
    display: none;
  }

  details[open] > #toggle:checked ~ * {
    display: unset;
  }

  /* If the browser supports details+summary, hide and don't bother with the checkbox */
  details[open] > #toggle { display: none !important }
</style>
```html
