# useful-code-snippets
A collection of useful code snippets

## run some function or code-block n number of times:
```
[...Array(n)].map()
```
Example:
```
const emptyRows = [ ...Array(5) ].map((_, i) => {
                return (
                    <tr key={`empty-row-${i}`}>
                        <td>&nbsp;</td>
                    </tr>
                )
            });
```

## combining `document.write` and script tags of `type=text/html` to dynamically load polyfill scripts
using `document.write`, particularly for script tags, will cause a blocking request.  This is useful for loading polyfills before loading the rest of the app code.  It is especially useful if you are doing a feature detection check before making the request to another file containing the polyfill code.  The location of the external file can be stored a script tag of type `text/html`

Example:
```
    // this first script is just text, this is prevent the browser from actually trying load the script file
    // think of it like a template file -- it will not get rendered to the browser due to the type = text/html
    <script id="object-assign-polyfill" type="text/html">
      <script src="PATH_TO_OBJECT_ASSIGN_POLYFILL.js" ></script>
    </script>
    
    // this 2nd script does the feature detection -- it will use document.write to dynamically 
    // request the polyfill script based on the featue detection check
    <script>
        (function () {
            if (!Object.assign) {
                document.write('<scr' + 'ipt src=' + document.getElementById('object-assign-polyfill').textContent.match(/src=\"(.*)\"/)[1] + '></scr' + 'ipt>');
            }
        })();
    </script>
```




