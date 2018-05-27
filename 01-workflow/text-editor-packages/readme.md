## Text Editors & Their Packages

Text editor packages help make our lives as developers easier by providing functionalities such as linter error detection, indentation correction, color preview, etc.


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

  * EditorConfig (reads configuration files for your editor)
  * ColorPicker (pick colors by typing `COMMAND + SHIFT + c`, handy for CSS)
  * Color Highlighter (visually displays colors for hex/rgb values)
  * GitGutter (shows git additions/deletions)
  * Terminal (launch a terminal window from a folder on the sidebar)
  * BracketHighlighter (highlight brackets and tabs)
  * Sass (syntax definition, we'll use this when working with Rails)
  * Babel (syntax definition, we'll use this when working with React)
  * JSX (syntax definition, we'll use this when working with React)
