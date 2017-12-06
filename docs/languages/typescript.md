---
Order: 6
Area: languages
TOCTitle: TypeScript
ContentId: 05C114DF-4FDC-4C65-8954-58F5F293FAFD
PageTitle: TypeScript Programming with Visual Studio Code
DateApproved: 5/4/2017
MetaDescription: Get the best out editing TypeScript with Visual Studio Code.
MetaSocialImage: typescript_Languages_typescript.png
---
# Editing TypeScript

TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.
It offers classes, modules, and interfaces to help you build robust components. A language specification can be found [here](https://github.com/Microsoft/TypeScript/tree/master/doc).

![TypeScript language within VS Code](images/typescript/typescript_hero.png)

VS Code's TypeScript support can operate in two different modes:

* **File Scope**: in this mode TypeScript files opened in Visual Studio Code are treated as independent units. As long as a file `a.ts` doesn't reference a file `b.ts` explicitly (either using [/// reference directives](https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html) or external modules) there is no common project context between the two files.

* **Explicit Project**: a TypeScript project is defined via a `tsconfig.json` file. The presence of such a file in a directory indicates that the directory is the root of a TypeScript project. The file itself lists the files belonging to the project as well as compiler options. Details about the `tsconfig.json` file can be found [here](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html).

>**Tip:** We recommend that you use explicit projects over file scope projects. Since explicit projects list the files belonging to a project language, features like `Find All References` `kb(editor.action.referenceSearch.trigger)` consider the project scope and not the file scope only.

## tsconfig.json

Typically the first step in any new TypeScript project is to add in a `tsconfig.json` file.  This defines the TypeScript [project settings](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) such as the compiler options and the files that should be included.  To do this, open up the folder where you want to store your source and add in a new file named `tsconfig.json`.  Once in this file, IntelliSense will help you along the way.

![jsconfig.json IntelliSense](images/typescript/jsconfigintellisense.png)

A simple `tsconfig.json` looks like this for ES5, **CommonJS** [modules](http://www.commonjs.org/specs/modules/1.0) and source maps:

```json
{
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "sourceMap": true
    }
}
```

Now when you create a `.ts` file as part of the project we will offer up rich editing experiences and syntax validation.

## Transpiling TypeScript into JavaScript

VS Code integrates with `tsc` through our integrated [task runner](/docs/editor/tasks.md).  We can use this to transpile `.ts` files into `.js` files.  Let's walk through transpiling a simple TypeScript Hello World program.

### Step 1: Create a simple TS file

Open VS Code on an empty folder and create a `HelloWorld.ts` file, place the following code in that file...

```ts
class Startup {
    public static main(): number {
        console.log('Hello World');
        return 0;
    }
}

Startup.main();
```

### Step 2: Create tasks.json

The next step is to set up the task configuration.  To do this open the **Command Palette** with `kb(workbench.action.showCommands)` and type in **Configure Task Runner**, press `kbstyle(Enter)` to select it. This shows a selection box with templates you can choose from:

![Task Runner Selection](images/typescript/taskSelection.png)

Select TypeScript - tsconfig.json. This will create a `tasks.json` file in the workspace `.vscode` folder.

The content of the tasks.json file looks like this:

```json
{
	// See https://go.microsoft.com/fwlink/?LinkId=733558
	// for the documentation about the tasks.json format
	"version": "0.1.0",
	"command": "tsc",
	"isShellCommand": true,
	"args": ["-p", "."],
	"showOutput": "silent",
	"problemMatcher": "$tsc"
}
```

> **Tip:** While the template is there to help with common configuration settings, IntelliSense is available for the `tasks.json` file as well to help you along.  Use `kb(editor.action.triggerSuggest)` to see the available settings.

Under the covers we interpret `tsc` as an external task runner exposing exactly one task: the compiling of TypeScript files into JavaScript files. The command we run is: `tsc -p .`

>**Tip:** If you don't have the TypeScript compiler installed, you can [get it here](https://www.typescriptlang.org/).

### Step 3: Run the Build Task

As this is the only task in the file, you can execute it by pressing `kb(workbench.action.tasks.build)` (**Run Build Task**).  At this point you will see an additional file show up in the file list `HelloWorld.js`.

The example TypeScript file did not have any compile problems, so by running the task all that happened was a corresponding `HelloWorld.js` and `HelloWorld.js.map` file was created.

If you have [Node.js](https://nodejs.org) installed, you can run your simple Hello World example by opening up a terminal and running:

```
node HelloWorld.js
```


> **Tip:** You can also run the program using VS Code's Run/Debug feature. Details about running and debugging node apps in VS Code can be found [here](/docs/nodejs/nodejs-tutorial.md#debugging-your-node-application)

### Step 4: Reviewing Build Issues

Unfortunately, most builds don't go that smoothly and the result is often some additional information.  For instance, if there was a simple error in our TypeScript file, we may get the following output from `tsc`:

    HelloWorld.ts(3,17): error TS2339: Property 'logg' does not exist on type 'Console'.

This would show up in the output window (which can be opened using `kb(workbench.action.output.toggleOutput)`) and selecting Tasks in the output view dropdown.  We parse this output for you and highlight detected problems in the Status Bar.

![Problems in Status Bar](images/typescript/problemstatusbar.png)

You can click on that icon to get a list of the problems and navigate to them.

![Compile Problems](images/typescript/compileerror.png)

You can also use the keyboard to open the list `kb(workbench.actions.view.problems)`.

>**Tip:** Tasks offer rich support for many actions. Check the [Tasks](/docs/editor/tasks.md) topic for more information on how to configure them.

## Goto Symbol & Show All Symbols

`kb(workbench.action.gotoSymbol)`: lists all defined symbols of the current open TypeScript and lets you navigate in it.

`kb(workbench.action.showAllSymbols)`: lets you search all symbols defined in the current project or file scope. You need to have a TypeScript file open in the active editor.

## Format Code

`kb(editor.action.formatDocument)`: formats the whole document.
`kb(editor.action.formatSelection)`: formats the currently selected source code.

## JSDoc Support

VS Code offers **JSDoc** support for TypeScript. Besides syntax coloring, we help you enter **JSDoc** comments. Type `/**` and it will auto insert the closing `*/`. Pressing `kbstyle(Enter)` inside a **JSDoc** block will indent the next line and auto insert a `*`.

## JavaScript Source Map Support

TypeScript debugging supports JavaScript source maps. Enable this by setting the `sourceMaps` attribute to `true` in the project's launch configuration file `launch.json`. In addition, you can specify a TypeScript file with the `program` attribute.

To generate source maps for your TypeScript files, compile with the `--sourcemap` option or set the `sourceMap` property in the `tsconfig.json` file to `true`.

In-lined source maps (a source map where the content is stored as a data URL instead of a separate file) are also supported, although in-lined source is not yet supported.

## Setting a different outFiles for generated files

If generated (transpiled) JavaScript files do not live next to their source, you can help the VS Code debugger locate them by setting the `outFiles` attribute in the launch configuration. Whenever you set a breakpoint in the original source, VS Code tries to find the generated source by searching the files specified by glob patterns in `outFiles`.

## Hiding Derived JavaScript Files

When you are working with TypeScript, you often don’t want to see generated JavaScript files in the explorer or in search results. VS Code offers filtering capabilities with a `files.exclude` [workspace setting](/docs/getstarted/settings.md) (**File** > **Preferences** > **Settings**) and you can easily create an expression to hide those derived files:

`"**/*.js": { "when": "$(basename).ts"}`

This pattern will match on any JavaScript file (`**/*.js`) but only if a sibling TypeScript file with the same name is present. The file explorer will no longer show derived resources for JavaScript if they are compiled to the same location.

![Hiding derived resources](images/typescript/hidingDerivedBefore.png) ![Hiding derived resources](images/typescript/hidingDerivedAfter.png)

To exclude JavaScript files generated from both `.ts` and `.tsx` source files, use this expression:

```json
"**/*.js": { "when": "$(basename).ts" },
"**/**.js": { "when": "$(basename).tsx" }
```

This is a bit of a trick. The search glob pattern is used as a key. The settings above use two different glob patterns to provide two unique keys but the search will still match the same files.

## Mixed TypeScript and JavaScript projects

It is now possible to have mixed TypeScript and JavaScript projects. To enable JavaScript inside a TypeScript project, you can set the `allowJs` property to `true` in the `tsconfig.json`.

>**Tip:** The `tsc` compiler does not detect the presence of a `jsconfig.json` file automatically. Use the `–p` argument to make `tsc` use your `jsconfig.json` file, e.g. `tsc -p jsconfig.json`.

## Using Newer TypeScript Versions

VS Code ships with a recent stable version of the TypeScript language service and it may not match the version of TypeScript installed globally on your computer or locally in your workspace. The active version of the TypeScript language service is displayed in the Status Bar when viewing a TypeScript or JavaScript file:

![TypeScript status bar version](images/typescript/status-bar-version.png)

When VS Code detects that the TypeScript compiler (tsc) version is different than the active TypeScript language service version, a message is displayed "Version mismatch! global tsc (2.1.5) != VS Code's language service (2.2.1). Inconsistent compiler errors might occur". This message is benign and is meant to alert the user to the possible differences between the compiler and active language service.

You can disable this check with the `typescript.check.tscVersion` user or workspace [setting](/docs/getstarted/settings.md). This will be set to false in your user settings if you click the **Don't Check Again** in the message banner.

```json
  "typescript.check.tscVersion": false
```

Another option is to install the matching version of TypeScript in your workspace (`npm install --save-dev typescript`) or globally  on your computer (`npm install -g typescript`). We recommend installing TypeScript locally to avoid possible interactions with other TypeScript projects you may have.

>**Tip:** To get a specific TypeScript version, specify `@version`. For example for TypeScript 2.2.1, you would use `npm install --save-dev typescript@2.2.1`. To preview the next version of TypeScript, run `npm install --save-dev typescript@next`.

As VS Code updates the TypeScript language service in subsequent releases, you may see the mismatch message again and want to refresh your installed version of TypeScript.

To use a different TypeScript version by default, configure `typescript.tsdk` in your user settings to point to a directory containing the TypeScript `tsserver.js` file. You can find the TypeScript installation location using `npm list -g typescript`. The `tsserver.js` file is usually in the `lib` folder.

For example:

```json
{
   "typescript.tsdk": "/usr/local/lib/node_modules/typescript/lib"
}
```

You can also configure a specific version of TypeScript in a particular workspace by adding a `typescript.tsdk` workspace setting pointing to the directory of the `tsserver.js` file:

```json
{
   "typescript.tsdk": "./node_modules/typescript/lib"
}
```

If your workspace has a specific TypeScript version, you can switch between the workspace version of TypeScript and the version that VS Code uses by default by opening a TypeScript or JavaScript file in the workspace and clicking on the TypeScript version number in the Status Bar. A message box will appear asking you which version of TypeScript VS Code should use:

![TypeScript version selector](images/typescript/select-ts-version-message.png)

After switching versions of TypeScript or changing `typescript.tsdk`, restart VS Code to have the change take effect. You can switch back to the version of TypeScript that comes with VS Code by clicking on the TypeScript version in the Status Bar again.

## References CodeLens

The TypeScript references CodeLens displays an inline count of reference for classes, interfaces, methods, properties, and exported objects:

![TypeScript references code lens](images/typescript/ts-references-code-lens.png)

You can enable this by setting `"typescript.referencesCodeLens.enabled": true`.

Click on the reference count to quickly browse a list of references:

![TypeScript references code lens peek](images/typescript/ts-references-code-lens-peek.png)


## TypeScript Extensions

VS Code provides many features for TypeScript out of the box. In addition to what comes built-in, you can install an extension for greater functionality.

<div class="marketplace-extensions-typescript-curated"></div>

> Tip: Click on an extension tile above to read the description and reviews to decide which extension is best for you. See more in the [Marketplace](https://marketplace.visualstudio.com).

## Next Steps

OK, read on to find out about:

* [JavaScript](/docs/languages/javascript.md) - we have several JavaScript specific features in VS Code
* [Tasks](/docs/editor/tasks.md) - we used tasks to transpile your TS file. Read more to find out what else tasks can do
* [Basic Editing](/docs/editor/codebasics.md) - Learn about the powerful VS Code editor.
* [Code Navigation](/docs/editor/editingevolved.md) - Move quickly through your source code.
* [Debugging](/docs/editor/debugging.md) - we support debugging TypeScript Node.js apps

## Common Questions

**Q: How do I resolve a TypeScript "Cannot compile external module" error?**

**A**: If you get that error, resolve it by creating a `tsconfig.json` file in the root folder of your project. The tsconfig.json file lets you control how Visual Studio Code compiles your TypeScript code. For more information, see the [tsconfig.json overview](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html).

Due to a current limitation, you must restart VS Code after adding the `tsconfig.json` file.

**Q: Can I use the version of TypeScript that ships with VS 2015?**

**A**: No, the TypeScript language service which ships with Visual Studio 2015 and 2017 isn't compatible with VS Code. You will need to install a separate version of TypeScript from [npm](https://www.npmjs.com/package/typescript).
