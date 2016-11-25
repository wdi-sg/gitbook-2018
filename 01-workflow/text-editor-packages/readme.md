## Text Editors & Their Packages

Text editor packages help make our lives as developers easier by providing functionalities such as linter error detection, indentation correction, color preview, etc.

## Atom
Atom comes built in with some packages (e.g. autocomplete, bracket-matcher, etc), and you can also install other useful packages. Today, we will install five additional packages which you will find helpful on your journey as a developer.

Note: There are many packages with **very** similar names (e.g. color-picker vs. colorpicker vs. color picker). Do look at the name of the package carefully before installing them.

1 . If you haven't already done so, download Atom at https://atom.io. Unzip the file. Paste the Atom application into your Applications folder.

2 . From the Atom editor menu, navigate to Atom -> Preferences (or press `⌘` + `,`).

3 . From there, click on the `+ Install` tab on the left navigation bar.

4 . Search for the following packages (you'll have to hit enter to run the search), and install them one at a time:
* emmet
* color-picker
* pigments
* linter-js-standard
* standard-formatter

5 . Key commands for these packages
* emmet
  * http://docs.emmet.io/cheat-sheet/
* color-picker
  * Hit `⌘` + `shift` + `C` to show colour. Repeat to scroll through different colours, and press any other key to exit.
* standard-formatter
  * Hit `Ctrl` + `Alt` + `F` to fix minor linter errors in your javascript file

---

## Brackets
  1 . On the menu, navigate to File -> Extension Manager.

  2 .  Search for the following packages (you'll have to hit enter to run the search), and install them one at a time:
  * Emmet
  * JSHint
  * Brackets Color Picker
    * type 'color:' to see color picker
  * Brackets Css Color Preview


---
## Sublime

  #### Setting up User Settings

  * Open Sublime Text
  * Go to `Sublime Text -> Preferences -> Settings - User`
  * Replace the file with the settings object below:

  ```
  {
    "rulers":
    [
      80
    ],
    "tab_size": 2,
    "translate_tabs_to_spaces": true,
    "scroll_past_end": true
  }
  ```

  #### Setting up Package Control in Sublime Text

  * Open Sublime Text
  * Bring up the console
    * Use CTRL + ` on OSX
    * or `View > Show Console`
  * Go to https://packagecontrol.io/installation and paste the appropriate code into your Terminal
    * You should be using Sublime Text 3, so copy the Sublime Text 3 code.
  * Restart Sublime

  #### Install Sublime Packages

  * Type `COMMAND + SHIFT + P` to open the Command Palette
    * `CTRL + SHIFT + P` on Linux
  * Type `Install Package` and select the first result (by pressing `ENTER`)
  * Type the package you want to install, and press `ENTER` to begin installation.

  #### Useful Packages that you should install

  * ColorPicker (pick colors by typing `COMMAND + SHIFT + c`, handy for CSS)
  * Color Highlighter (visually displays colors for hex/rgb values)
  * EditorConfig (reads configuration files for your editor)
  * GitGutter (shows git additions/deletions)
  * Terminal (launch a terminal window from a folder on the sidebar)
  * BracketHighlighter (highlight brackets and tabs)
  * Bootstrap 3 Snippets (tab snippets for Bootstrap 3 elements)
  * EJS (syntax definition, we'll use this when working with Node)
  * Sass (syntax definition, we'll use this when working with Rails)
  * Babel (syntax definition, we'll use this when working with React)
  * JSX (syntax definition, we'll use this when working with React)

  #### Creating a Snippet (Optional)

  We'll use a lot of snippets when working with Bootstrap, and you can make your own as well.

  * Go to `Tools > New Snippet`
  * Include the content of your snippet inside `<![CDATA[ ]]>` within the `<content>` element.
  * To define how to trigger the snippet, uncomment the `<tabTrigger>` line and type the keyword for your tab trigger.
  * To trigger the snippet only on certain files (for example, only HTML, or only JavaScript), uncomment the `<scope>` tag and change the scope to the language you need.
  * More details and advanced functionality can be found in [this handy blog post](http://www.hongkiat.com/blog/sublime-code-snippets/)
