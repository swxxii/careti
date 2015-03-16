# Integration Examples #

First, download and include the script in your page header.

```
<script type="text/javascript" src="careti.js">
```

Careti is neatly wrapped up in the `CARETI` object. There are a few different ways to attach the correction features to a `<textarea>` element. You need to insert the appropriate function calls at the end of the main `careti.js` script, include as a separate/inline `<script>`.

### Passive Attachment ###

This attaches once to every `<textarea>` in the document. Use for static pages.
```
CARETI.init(); // first you must initialise
CARETI.attachBody('textarea'); // attach to every textarea in document
```

Note the flexibility to specify the element type. You could also attach to `<input>` type elements although the behaviour is not ideal (see the [quirks](UsingCareti.md) page).


### Aggressive/IFrame Attachment ###

The aggressive attachment mode polls document for changes and is therefore more expensive. It should only be used on dynamic pages, or pages containing `<iframe>` elements. This is the behaviour of the Greasemonkey version of the script.

```
// no initialisation needed for polling version
CARETI.attachPoller(1000); // poll interval of one second
```

The aggressive mode also checks inside `<iframe>` elements but only one level deep. That is, `iframes` inside `iframes` will not be found. This is for performance reasons and simplicity of code. For example, consider the document structure below: `textarea#1` _will_ be found by the script, but `textarea#2` will not.

```
<iframe>
  <html>
    <body>
      <textarea id="1" />
      <iframe>
        <html>
          <body><textarea id="2" /></body>
        </html>
      </iframe>
    </body>
  </html>
</iframe>
```

You may think the code below should do the job for relatively static pages containing `<iframe>` elements.

```
CARETI.init(); // initialise first
CARETI.attachInsideIframes('textarea');
```

However, my testing has shown that the `<iframe>` contents will not be loaded when `attachInsideIframes` is called, so best to use the document polling method (aggressive).

### Attaching to individual elements ###

For JavaScript developers, Careti can also be attached to individual elements retrieved from the DOM. For example:

```
// initialise first
var elem = document.getElementById('my_element');
CARETI.addEventListeners(elem);
```

This will add the required event listeners, enabling auto correction on the element you retrieved. Obviously you'll only want to attach to elements the user types into. The `addEventListeners` function can also be used within `for` loops after retrieving a set of elements, or anywhere else you like.