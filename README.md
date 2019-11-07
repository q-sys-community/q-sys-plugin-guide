# QSC Q-Sys Plugin Development Guide

Development Template & Guides for building and deploying QSC Q-Sys Community Plugins.

[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)
[![LinkedIn](https://img.shields.io/badge/contact-LinkedIn-blue)](https://www.linkedin.com/company/soloworkslondon/)

A resource for development of plugins for the Q-Sys platform.

- [QSC Q-Sys Plugin Development Guide](#qsc-q-sys-plugin-development-guide)
  - [Getting Started](#getting-started)
    - [Notes about using from source vs installing a version](#notes-about-using-from-source-vs-installing-a-version)
    - [Quickstart for Use](#quickstart-for-use)
    - [QuickStart for Development](#quickstart-for-development)
  - [Development](#development)
    - [Programming Environment (IDE)](#programming-environment-ide)
    - [Deployment](#deployment)
  - [Plugin File (LUA as .qplug)](#plugin-file-lua-as-qplug)
    - [GetControlLayout - Graphics Table](#getcontrollayout---graphics-table)
      - [Images](#images)
        - [Detail](#detail)
      - [Usage](#usage)
    - [Text Field](#text-field)
      - [Detail](#detail-1)
      - [Usage](#usage-1)
  - [NuSpec File Structure](#nuspec-file-structure)
    - [MetaData](#metadata)
      - [Author](#author)


## Getting Started

### Notes about using from source vs installing a version

Plugins are identified by NuGet by the ID and version in the NuSpec file, and by Q-Sys Designer by the UID. You can have multiple versions of a plugin, but if two have the same UID you will only see one.

For any live projects, install and use a versioned plugin.

For development, use the source and be wary of unpredictable results. You will also need to delete and re-drag the plugin after any significant changes to any non-control code to re-trigger compilation of correct hooks, otherwise results will be unpredictable.

### Quickstart for Use

- Open Q-Sys Designer with the `/dev` command line argument
- Under Asset Manager there will now be a cog icon active at the top right
- Add the community server `https://q-sys.soloworks.co.uk/q-sys-community-plugins/`
- **Note:** Entries may double up on first time, refresh view with :arrows_counterclockwise: to clear

### QuickStart for Development

Some knowledge of Git based workflows is required, to submit changes and improvements please follow standard fork/branch workflow and submit a Pull Request when finished.

Clone a plugin repository to somewhere (we use Documents/GitHub/repo-name)

```ps2
git clone github.com/q-sys-community/q-sys-plugin-<reponame>
```

User Defined plugins are defined and placed in the folowing folder. Copy the .qplug file from the content folder to here

```cmd
%userprofile%\Documents\QSC\Q-Sys Designer\Plugins
```

This will now appear under user plugins as version 0.0.0.0-master. When finished with changes, copy back to the repo and commit.

**Note:** Please only submit tested and working files, always ensure they have been linted with the correct extensions (see below) to ensure only code changes are submitted and not syntax and spacing. We are opinionated with code in order to debug logic, not spacing.

## Development

### Programming Environment (IDE)

For consistency of coding structure, Visual Studio Code should be used for development with the following extensions:

-  `Markdown All in One Extension` (Search String: `yzhang.markdown-all-in-one`)
-  `vscode-lua` (Search String: `trixnz.vscode-lua`)

Plugin files with extension .qplug can be associated with Visual Studio Code by editing settings.json:

```json
{
    "files.associations": {
        "*.qplug": "lua"
    }
}
```
When saving .lua files, always automatically format the file, either by right-clicking the code and chosing `Format Document` or with the shortcut  (Shift-Alt-F). This will help to prevent git code changes flagged based on whitespace or code style.

### Deployment

All plugins are open source, which means you are free to dig in and contribute. To submit changes, you can fork a branch and when done submit a pull request. Once accepted, a plugin will be tagged with a new version number (in the format x.x.x.x), and will be automatically packaged and pushed to the server from which it can be installed and used.

## Plugin File (LUA as .qplug)

### GetControlLayout - Graphics Table

#### Images

##### Detail

Images can be included by encoding Svg, Png or Jpeg as a base64 string using a site such as:

- https://www.base64-image.de/
- https://www.browserling.com/tools/image-to-base64

For example of the encoded string provided, examine the raw source of this file. The image below is embeded as 

```
data:image/png;base64,<base64EncodedTextHere>
```
and produces this image

![Hello World](data:image/png;base64,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)

#### Usage

```lua
MyBase64EncodedImage = "iVBORw0KGgoAAAANS..." -- Truncated Base64

{
    Type = "Svg",   -- SVG
    --Type = "Image", -- PNG,JPEG
    Image = MyBase64EncodedImage,
    Position = {x, y},
    Size = {x, y}
}
```

### Text Field

#### Detail

Used to display dynamic text

#### Usage

```lua
{
    Style = "Text",
    HTextAlign = "Left",
    Padding = i,
    StrokeWidth = i,
    Position = {x, y},
    Size = {x, y}
}
```
## NuSpec File Structure

### MetaData

#### Author

- "QSC" will cause plugin to appear in "QSC Managed" tree
- Anything else will appear in "User" tree
