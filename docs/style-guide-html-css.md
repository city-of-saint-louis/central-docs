# Style Guides : HTML/CSS

## HTML

``` html title="Tags are lowercase"
<!-- Incorrect -->
<img src="/images/image.png" alt="Picture of a flower" 
    title="Picture of a flower">
<!-- Correct -->
<img src="/images/image.png" alt="Picture of a flower" 
    title="Picture of a flower">
```

``` html title="Escape special characters"
<!-- Incorrect -->
http://example.com/search?name=detail&uid=165
<!-- Correct -->
http://example.com/search?name=detail&amp;uid=15
```

``` html title="Use a new line for every block, list, or table element, and indent every such child element"
<blockquote>
    <p>
        <em>Space</em>, the final frontier.
    </p>
</blockquote>
<ul>
    <li>Moe</li>
    <li>Larry</li>
    <li>Curly</li>
</ul>
<table>
    <tr>
        <th>Income</th>
        <th>Taxes</th>
    </tr>
    <tr>
        <td>$5.00</td>
        <td>$4.40</td>
    </tr>
</table>
```