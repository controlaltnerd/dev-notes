# Markdown Basics

| Type       | Or         | ...to Get |
| ---------- | ---------- | --------- |
| <code class="md-ref">\*Italic\*</code> | <code class="md-ref">\_Italic\_</code> | _Italic_ |
| <code class="md-ref">\*\*Bold\*\*</code> | <code class="md-ref">\_\_Bold\_\_</code> | **Bold** |
| <code class="md-ref"># Heading 1</code> | <code class="md-ref">Heading 1</code><br><code class="md-ref">=========</code> | # Heading 1 |
| <code class="md-ref">## Heading 2</code> | <code class="md-ref">Heading 2</code><br><code class="md-ref">---------</code> | ## Heading 2 |
| <code class="md-ref">\[Link](https://a.com)</code> | <code class="md-ref">[Link][1]</code><br><code class="md-ref">⋮</code><br><code class="md-ref">[1]: https://a.com</code> | [Link](https://a.com) |
| <code class="md-ref">\!\[Image](https://a.com/b.png)</code> | <code class="md-ref">![Image][1]</code><br><code class="md-ref">⋮</code><br><code class="md-ref">[1]: https://a.com/b.png</code> | ![Image](https://commonmark.org/help/images/favicon.png) |
| <code class="md-ref">> Blockquote</code> | | <blockquote>Blockquote</blockquote> |
| <code class="md-ref">* List</code><br><code class="md-ref">* List</code><br><code class="md-ref">* List</code> | <code class="md-ref">- List</code><br><code class="md-ref">- List</code><br><code class="md-ref">- List</code> | * List<br>* List<br>* List |
| <code class="md-ref">Horizontal Rule</code><br><br><code class="md-ref">---</code> | <code class="md-ref">Horizontal Rule</code><br><br><code class="md-ref">***</code> | Horizontal Rule<br><hr> |
| \`Inline code\` with backticks | | <code>Inline code</code> with backticks |
| \`\`\`<br># code block<br>print \`3 backticks or\`<br>print \`indent 4 spaces\`<br>\`\`\` | # code block<br>print \`3 backticks or\`<br>print \`indent 4 spaces\` | <pre><code># code block<br/>print \`3 backticks or\`<br/>print \`indent 4 spaces\`<code><pre>

## Keyboard Keys

To display keyboard keys as a special mark, wrap text with `<kbd></kbd>`:

```
Press <kbd>F1</kbd> at any time for help.
```

Press <kbd>F1</kbd> at any time for help.

## Tools

- [StackEdit.io](https://stackedit.io) - In-browser Markdown editor
- [Clipboard2Markdown](https://euangoddard.github.io/clipboard2markdown/) - Paste to Markdown