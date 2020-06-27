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
using `document.write`, particularly for script tags, will cause a blocking request.  This is useful for loading polyfills before loading the rest of the app code.  It is especially useful if you are doing a feature detection check before making the request to another file containing the polyfill code.  The location of the external file can be stored as a script tag of type `text/html`

Example:
```
    // this first script is just text, this is prevent the browser from actually trying load the script file
    // think of it like a template file -- it will not get rendered to the browser due to the type = text/html
    <script id="object-assign-polyfill" type="text/html">
      <script src="PATH_TO_OBJECT_ASSIGN_POLYFILL.js" ></script>
    </script>
    
    // this 2nd script does the feature detection -- it will use document.write to dynamically 
    // request the polyfill script based on the feature detection check
    <script>
        (function () {
            if (!Object.assign) {
                document.write('<scr' + 'ipt src=' + document.getElementById('object-assign-polyfill').textContent.match(/src=\"(.*)\"/)[1] + '></scr' + 'ipt>');
            }
        })();
    </script>
```

## Methods to dedup arrays, i.e. get an unique array
### With primitive values
#### Reduce
```
['come', 'on', 'come', 'in'].reduce((acc, name) => {
  if (!acc.includes(name)) {
    acc.push(name);
  }

  return acc;
}, []);  // ["come", "on", "in"]
```
#### Filter
```
['come', 'on', 'come', 'in'].filter((el, index, arr) => {
  return arr.indexOf(el) === index;
});
```
#### Set and spread operator
```
[...new Set(['come', 'on', 'come', 'in'])]
```
### With objects
#### Distinct property values of an array of objects
```
const array = [
  { id: 1, author: 'gabbi', body: 'react' },
  { id: 2, author: 'alice', body: 'angular' },
  { id: 3, author: 'gabbi', body: 'react again' }
];

const uniqueAuthors = [...new Set(array.map(x => x.author))]; // ['gabbi', 'alice']
```
#### With filter, retaining original object structure
```
const array = [
  { id: 3, name: 'Central Microscopy', fiscalYear: 2018 },
  { id: 5, name: 'Crystallography Facility', fiscalYear: 2018 },
  { id: 3, name: 'Central Microscopy', fiscalYear: 2017 },
  { id: 5, name: 'Crystallography Facility', fiscalYear: 2017 }
];

const byName = array.map(item => item.name);

array.filter((item, index) => byName.indexOf(item.name) === index);
// returns
// [{ id: 3, name: "Central Microscopy", fiscalYear: 2018 },
// { id: 5, name: "Crystallography Facility", fiscalYear: 2018 }]
```
