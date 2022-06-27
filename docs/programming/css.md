## Center a button inside a div
https://www.w3schools.com/howto/howto_css_center_button.asp

```css
@media only screen and (min-width: 768px) {
    .container {
        position: relative;
    }
    .vertical-center {
        position: absolute;
        top: 40%;
    }
}
```

## Pure-css
Putting a text element (ex: label) inside a `pure-g` directly makes it inherits a negative `letter-spacing` and makes the text mkhalat (overlap), it also happens if we don't set a `pure-u` class for a breakpoint (only tested with phones)

## Wtforms SelectField with int values
Add `coerce=int` for god's sake!
