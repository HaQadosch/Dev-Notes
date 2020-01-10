# Code Snippets

Quick write up of the [manual](https://code.visualstudio.com/docs/editor/userdefinedsnippets)

`File > Preferences > User Snippets`

It creates a json file in the root directory of the project `.vscode/name.code-snippets`

## Format
The format is:
```json
{
  "Print to console": {
    "scope": "javascript,typescript",
    "prefix": "log",
    "body": [
      "console.log('$1');",
      "$2"
    ],
    "description": "Log output to console"
  }
}
```

  * `Print to console` name of the snippet
  * `scope` the type of files where the snippet is valid, we use **typescriptreact** most of the time
  * `prefix` the set of letters that triggers the snippet
  * `body` the actual code of the snippet
  * `description` for the readers

## Tokens

`$1` and `$2` in the example below are tokens, names that need to be replaced by the user.
The caret will be focused automatically on `$1` pressing TAB will move the caret at `$2` and so on.

You can name the tokens:
```json
{
  "Print to console": {
    "scope": "javascript,typescript",
    "prefix": "log",
    "body": [
      "console.log('${1:message}');",
      "${2:alert}"
    ],
    "description": "Log output to console"
  }
}
```

`$0` is a special token, it specifies where the caret should land once you're done renaming all the tokens.

```json
"body": [
  "for (const ${2:item} of ${1:array}) {",
  "\t$0",
  "}"
],
```
Now, after the snippet tokens have been replaced, the cursor will be positioned inside the loop, preceded by a tab character.

## Predefined Token Values

  * `TM_SELECTED_TEXT` The currently selected text or the empty string
  * `TM_CURRENT_LINE` The contents of the current line
  * `TM_CURRENT_WORD` The contents of the word under cursor or the empty string
  * `TM_FILENAME` The filename of the current document
  * `TM_FILENAME_BASE` The filename of the current document without its extensions
  * `TM_DIRECTORY` The directory of the current document
  * `CLIPBOARD` The contents of your clipboard

## Example


```json
{
	"React Functional Component": {
		"scope": "typescriptreact",
		"prefix": "xbrfc",
		"body": [
			"import React from 'react'",
			"",
			"interface I${TM_FILENAME_BASE} {",
			"\t$0",
			"}",
			"",
			"export const ${TM_FILENAME_BASE}: React.FC<I${TM_FILENAME_BASE}> = () => {",
			"",
			"  return (",
			"    <div>",
			"",
			"    </div>",
			"  )",
			"}"
		],
		"description": "Create new React FC based on the filename"
	},
}
```