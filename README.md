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
