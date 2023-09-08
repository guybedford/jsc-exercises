# JS Component Exercises

## Setup

Install Node.js & npm (skip if already installed): https://nodejs.org/
For Windows / Mac -> Click “20.0.6 Current”
For linux: Select “Other downloads” -> Then “Installing Node.js via package manager”

Clone this repo, and run npm installs:

```
npm install
npm install -g @bytecodealliance/jco
npm install -g @bytecodealliance/componentize-js
```

Simple verification run:

```
jco run commands/chara.wasm say hello components
```

## Folder Layout

* `commands`: Command components
* `reactors`: Reactor components, with .wit files
* `reactors-resources`: Reactor components for resources exercises, with .wit files

## Project Links

* jco: https://github.com/bytecodealliance/jco
* ComponentizeJS: https://github.com/bytecodealliance/componentize-js

## Exercises

1. [Transpile Exercises](EXERCISES-TRANSPILE.md)
2. [Resources Exercises](EXERCISES-RESOURCES.md)
3. [ComponentizeJS exercises](EXERCISES-COMPONENTIZE.md)
