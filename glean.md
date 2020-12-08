 ![](https://github.com/wix/vscode-glean/raw/master/assets/github_logo.png?raw=true)

##**Glean**
___
>extension, React, Refactoring tool

allows easy extraction of JSX into new React components, either locally or into seperate files.

- Allows extracting JSX into new component
- Allows converting Class Components to Functional Components and vice-verse
- Allows wrapping JSX with conditional
- Allows renaming state variables and their setters simultaneously.
- Allows wrapping code with useMemo, useCallback or useEffect
- Moving code between files
- Typescript support
- ES2015 modules support
- CommonJS modules support

**Configuration Options**

glean.jsModuleSystem `(Default: 'esm')`
Determines how the selected code will be exported/imported. Valid options are 'esm' and 'commonjs'.

glean.jsFilesExtensions `(Default: [ "js", "jsx", "ts", "tsx" ])`
List of extensions of files that should be treated as javascript files. This determines whether or not the snippet will be exported and imported. The snippet will be treated as javascript only if the extension of both origin and target files appears in this list.

glean.switchToTarget `(Default: false)`
Determines whether VSCode should switch to target file after extracting.

glean.experiments `(Default: [])`
A list of enabled experimental features. Available experimental features:

glean.showConversionWarning `(Default: true)`