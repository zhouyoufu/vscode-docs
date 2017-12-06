---
Order: 11
Area: editor
TOCTitle: Creating your own Snippets
ContentId: 79CD9B45-97FF-48B1-8DD5-2555F56206A6
PageTitle: Creating your own Snippets in Visual Studio Code
DateApproved: 5/4/2017
MetaDescription: It is easy to add code snippets to Visual Studio Code both for your own use or to share with others on the public Extension Marketplace. TextMate .tmSnippets files are supported.
---
# Creating your own Snippets

Code snippets are templates that make it easier to enter repeating code patterns, such as loops or conditional-statements.

Snippets show in IntelliSense (`kb(editor.action.triggerSuggest)`) mixed with other suggestions as well as in a dedicated snippet picker (**Insert Snippet** in the Command Palette). There is also support for tab-completion: Enable it with `"editor.tabCompletion": true`, type a *snippet prefix*, and press `kb(insertSnippet)` to insert a snippet.

The snippet syntax follows the [TextMate snippet syntax](https://manual.macromates.com/en/snippets) with the exception of 'regular expression replacements', 'interpolated shell code' and 'transformations', which are not supported.

<video id="snippets-showcase" src="https://az754404.vo.msecnd.net/public/snippets_showcase.mp4" placeholder="/images/userdefinedsnippets_snippets_placeholder.png" autoplay loop controls muted>
    Sorry you're browser doesn't support HTML 5 video.
</video>

## Add Snippets from the Marketplace

Many [extensions](/docs/editor/extension-gallery.md) on the VS Code [Marketplace](https://marketplace.visualstudio.com/vscode) include snippets.  If you find one you want to use, install it and restart VS Code and the new snippet will be available (see [here](/docs/editor/extension-gallery.md#browse-and-install-extensions) for more instructions on installing an extension).

Below are some popular extensions which include snippets in their language support:

<div class="marketplace-extensions-snippets"></div>

> Tip: The extensions shown above are dynamically queried. Click on an extension tile above to read the description and reviews to decide which extension is best for you. See more in the [Marketplace](https://marketplace.visualstudio.com/vscode).

## Creating your Own Snippets

You can define your own snippets for specific languages.  To open up a snippet file for editing, open **User Snippets** under **File** > **Preferences** (**Code** > **Preferences** on Mac) and select the language for which the snippets should appear.

Snippets are defined in a JSON format and stored in a per user `(languageId).json` file. For example, Markdown snippets go in a `markdown.json` file.

The example below is a `For Loop` snippet for `JavaScript`.

```json
    "For Loop": {
        "prefix": "for",
        "body": [
            "for (var ${1:index} = 0; ${1:index} < ${2:array}.length; ${1:index}++) {",
            "\tvar ${3:element} = ${2:array}[${1:index}];",
            "\t$0",
            "}"
        ],
        "description": "For Loop"
    },
```

In the example above:

* `For Loop` is the snippet name
* `prefix` defines how this snippet is selected from IntelliSense and tab completion. In this case `for`.
* `body` is the content and either a single string or an array of strings of which each element will be inserted as separate line.
* `description` is the description used in the IntelliSense drop down

The example above has three placeholders, `${1:index}`, `${2:array}`, and `${3:element}`. You can quickly traverse them in the order of their number. The string after the number and colon is filled in as default.

Once you have added a new snippet, you can try it out right away, no restart needed.

## Snippet Syntax

The `body` of a snippet can use special constructs to control cursors and the text being inserted. The following are supported features and their syntaxes:

### Tabstops

With tabstops you can make the editor cursor move inside a snippet. Use `$1`, `$2` to specify cursor locations. The number is the order in which tabstops will be visited, whereas `$0` denotes the final cursor position. Multiple tabstops are linked and updated in sync.

### Placeholders

Placeholders are tabstops with values, like `${1:foo}`. The placeholder text will be inserted and selected such that it can be easily changed. Placeholders can be nested, like `${1:another ${2:placeholder}}`.

### Variables

With `$name` or `${name:default}` you can insert the value of a variable. When a variable isn’t set its *default* or the empty string is inserted. When a variable is unknown (that is, its name isn’t defined) the name of the variable is inserted and it is transformed into a placeholder. The following variables can be used:

* `TM_SELECTED_TEXT` The currently selected text or the empty string
* `TM_CURRENT_LINE` The contents of the current line
* `TM_CURRENT_WORD` The contents of the word under cursor or the empty string
* `TM_LINE_INDEX` The zero-index based line number
* `TM_LINE_NUMBER` The one-index based line number
* `TM_FILENAME` The filename of the current document
* `TM_DIRECTORY` The directory of the current document
* `TM_FILEPATH` The full file path of the current document


### Grammar

Below is the EBNF for snippets. With `\` (backslash) you can escape `$`, `}` and `\`.

```
any         ::= tabstop | placeholder | variable | text
tabstop     ::= '$' int | '${' int '}'
placeholder ::= '${' int ':' any '}'
variable    ::= '$' var | '${' var }' | '${' var ':' any '}'
var         ::= [_a-zA-Z] [_a-zA-Z0-9]*
int         ::= [0-9]+
text        ::= .*
```

## Using TextMate Snippets

You can also use existing TextMate snippets (.tmSnippets) with VS Code. See the [Using TextMate Snippets](/docs/extensions/themes-snippets-colorizers.md#using-textmate-snippets) topic in our Extension Authoring section to learn more.

## Next Steps

* [Command Line](/docs/editor/command-line.md) - VS Code has a rich command line interface to open or diff files and install extensions.
* [Extending Visual Studio Code](/docs/extensions/overview.md) - Learn about other ways to extend VS Code.
* [Themes, Snippets, and Colorizers](/docs/extensions/themes-snippets-colorizers.md) - You can package themes, snippets and language colorizers for use in VS Code.

## Common Questions

**Q: What if I want to use existing TextMate snippets from a .tmSnippet file?**

**A:** You can easily package TextMate snippets files for use in VS Code. See [Using TextMate Snippets](/docs/extensions/themes-snippets-colorizers.md#using-textmate-snippets) in our Extension Authoring documentation.

