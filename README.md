# QSC Q-Sys Plugin Development Guide <!-- omit in toc -->

Development Template & Guides for building and deploying QSC Q-Sys Community Plugins.

[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)
[![LinkedIn](https://img.shields.io/badge/Contact-LinkedIn-blue)](https://www.linkedin.com/company/soloworkslondon/)

A resource for development of plugins for the Q-Sys platform.

---

- [Getting Started](#getting-started)
  - [Quickstart for Use](#quickstart-for-use)
  - [QuickStart for Development](#quickstart-for-development)
- [Development](#development)
  - [Overview](#overview)
    - [Setup](#setup)
    - [Control Code](#control-code)
  - [Programming Environment (IDE)](#programming-environment-ide)
  - [Deployment](#deployment)
  - [Source Control](#source-control)
- [Plugin File Structure (.lua as .qplug)](#plugin-file-structure-lua-as-qplug)
  - [*Local Table:* PluginInfo](#local-table-plugininfo)
  - [*Function:* GetProperties() *returns* Table](#function-getproperties-returns-table)
  - [*Function:* RectifyProperties(Table) *returns* Table](#function-rectifypropertiestable-returns-table)
  - [*Global Table:* Properties](#global-table-properties)
  - [*Function:* GetPrettyName(Table) *returns* String](#function-getprettynametable-returns-string)
  - [*Function:* GetControls(Table) *returns* Table](#function-getcontrolstable-returns-table)
  - [*Global Table:* Controls](#global-table-controls)
  - [*Function:* GetPages(Table) *returns* Table](#function-getpagestable-returns-table)
  - [*Function:* GetComponents(Table) *returns* Table](#function-getcomponentstable-returns-table)
  - [*Function:* GetControlLayout(Table) *returns* Table,Table](#function-getcontrollayouttable-returns-tabletable)
    - [Graphics Table: Images](#graphics-table-images)
    - [Graphics Table: Text Field](#graphics-table-text-field)
- [NuSpec File Structure](#nuspec-file-structure)
  - [MetaData](#metadata)
    - [Author](#author)
  - [Description](#description)
  - [Release Notes](#release-notes)

---

## Getting Started

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

---

## Development

### Overview

Q-Sys Plugins are single file LUA scripts built around a specific framework. They are delivered using [NuGet](https://www.nuget.org/) based system.

Plugins are managed in your file system by NuGet using the ID and version in the format `id.0.0.0.0`, and by Q-Sys Designer by the `PluginInfo.Id` value. This means you can have multiple versions of a plugin installed, but if they are not given unique ids you may only see one.

**Do not work on plugins directly from the installed files**, instead make a copy of the plugin from the Repo and work in your plugins folder. Always delete and re-load a plugin after making changes to anything except the control code body, and even then if you encounter behaviour that you don't understand, in the first instance re-load designer and re-drag your plugin.

Plugins consist of two clear sections, which need to be understood to avoid confusion:

#### Setup

This section consists of all Plugin configuration:

- PluginInfo - System infomation and MetaData
- Properties - Configurable values presented in Properties Pane
- Controls - Input / Output GUI objects and/or PINs
- GUI Layout - Look and Feel of the presented UI pages

When a plugin is added to a schematic, the above areas are compiled into your program at that point in time. Any changes will not be reflected until either deletion of the control, or possibly a re-load of designer, and will result in very unpredictable results.

#### Control Code

This section consists of all control code, setting and reading Controls and Properties. This is where the actual moving parts go, changes to this section are picked up on every re-compile or emulation.

### Programming Environment (IDE)

For consistency of coding structure, Visual Studio Code should be used for development with the following extensions:

- `Markdown All in One Extension` (Search String: `yzhang.markdown-all-in-one`)
- `markdownlint` (Search String: `davidanson.vscode-markdownlint`)
- `vscode-lua` (Search String: `trixnz.vscode-lua`)

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

### Source Control

Use the following guides when making commits to maintain a standard structure and keep automation easier:

- How to Write a Git Commit Message: <https://chris.beams.io/posts/git-commit/>
- Conventional Commits: <https://www.conventionalcommits.org/en/v1.0.0-beta.4/>

In summary, the rules to adhere to are:

- `<type>: <description in 50 chars>`
- `<type>` can be one of:
  - fix
  - feat
  - docs
  - style
  - refactor
  - chore
- Do not end with a `"."`
- Use the imperative mood (Spoken or written as if giving a command or instruction) e.g.
  - Update document to show...
  - Stop function returning...
  - Alter workflow for control of...
- Commit message body is optional, but should be sensible, concise and descriptive if used

---

## Plugin File Structure (.lua as .qplug)

Plugin files are single file LUA scripts with a specific Q-Sys flavour framework. They can have the extension .lua, but convention is to use .qplug.

They consist of the following user editable elements:

- *Local Table:* PluginInfo
- *Global Table:* Controls
- *Function:* GetProperties() *returns* Table
- *Global Table:* Properties
- *Function:* RectifyProperties(Table) *returns* Table
- *Function:* GetPrettyName(Table) *returns* String
- *Function:* GetControls(Table) *returns* Table
- *Function:* GetPages(Table) *returns* Table
- *Function:* GetControlLayout(Table) *returns* Table,Table
- *Function:* GetComponents(Table) *returns* Table
- *Body:* User control code, wrapped in `if Controls`

---

### *Local Table:* PluginInfo

Contains infomation instructing Q-Sys Designer on how to handle the plugin.

Example:

```lua
PluginInfo = {
  -- Name of plugin in tree
  -- `~` allows for folder structure
  Name = "FolderName~PluginName",
  -- Version String (unused directly by Q-Sys Designer)
  Version = "VersionString",      // Version String
  -- Id is a UID for this plugin within the context of Designer
  Id = "qsysc.make.model.version",
  -- Description
  Description = "DescriptionString",
  -- Author (Used to put into group)
  -- QSC = QSC Managed
  -- Default: User
  Author = "AuthorString"
  -- Show Debug Window
  ShowDebug = true | false,
}
```

---

### *Function:* GetProperties() *returns* Table

Function to build and store the Properties table (see below)

```lua
function GetProperties()
  props = {
    -- Build as per Properties Table spec below
  }

  return props
end

```

---

### *Function:* RectifyProperties(Table) *returns* Table

Function to change properties table based on other properties. Called on any change of properties values.

```lua
  function RectifyProperties(props)
    -- Amend as per Properties Table spec below
    return props
  end
```

---

### *Global Table:* Properties

```lua
Properties = {
  { -- for integer property types
    Name = "MyIntegerName",
    Type = "integer",
    Min = 0,        -- Minimum Value
    Max = 10,       -- Maximum Value
    Value = 5,      -- Default Value
  },
  { -- for boolean property types
    Name = "MyBooleanName",
    Type = "boolean",
    Value = true | false, -- Default State
  },
  { -- for enum property types
    Name = "property_name",
    Type = "enum",
    Choices = {     -- List of options
      "Option_01",
      "Option_02"
      },
    Value = "Option_01",  -- Default Option
  },
}
```

---

### *Function:* GetPrettyName(Table) *returns* String

Return the string to display on the plugin control on the schamtic canvas. If not present will default to Id.

```lua
function GetPrettyName()
  return "My Friendly Name String V1.0.0.2"
end
```

---

### *Function:* GetControls(Table) *returns* Table

Return the Controls table as per Controls table spec below

```lua
function GetControls(props)
  ctls = {
    -- Build as per Controls Table spec below
  }
  return ctls
end
```

---

### *Global Table:* Controls

```lua
Controls = {
  {
    -- Button
    {
    Name = "control_name"
    ControlType = "Button"
    ButtonType = "Momentary" | "Toggle" | "Trigger",
    Count = integer, -- if > 0 this will make the control an array
    PinStyle = "Input" | "Output" | "Both",
    UserPin = true | false
    },
    -- Knob
    {
    Name = "control_name",
    ControlType = "Knob",
    ControlUnit = "dB" | "Hz" | "Float" | "Integer" | "Pan" | "Percent" | "Position" | "Seconds",
    Min = value,
    Max = value,
    Count = integer, -- if > 0 this will make the control an array
    PinStyle = "Input" | "Output" | "Both",
    UserPin = true | false
    },
    -- Indicator
    {
    Name = "control_name",
    ControlType = "Indicator",
    IndicatorType = "Led" | "Meter" | "Text" | "Status",
    Count = integer, -- if > 0 this will make the control an array
    PinStyle = "Input" | "Output" | "Both",
    UserPin = true | false
    },
    -- Text
    {
    Name = "control_name",
    ControlType = "Text",
    Count = integer, -- if > 0 this will make the control an array
    PinStyle = "Input" | "Output" | "Both",
    UserPin = true | false
    },
  }
}
```

---

### *Function:* GetPages(Table) *returns* Table

Return table of pages, allowing UI to be tabbed.

```lua
local pagenames = {"Mixer","Video Switcher"}

function GetPages(props)
  pages = {}
  if props["Model"].Value=="Model 1" then
    .insert(pages, {name = pagenames[1]}) -- Only the "Mixer" pages shows for Model 1
  else
    for ix,name in ipairs(pagenames) do
      table.insert(pages,{ name = pagenames[ix] }) -- All pages in 'pagenames' show for Model 2
    end
  end
  return pages
end
```

---

### *Function:* GetComponents(Table) *returns* Table

Store data here that isn't lost at runtime? (ToDo: Requires testing to see how it behaves)

```lua
function GetComponents(props)  
  return {}
end
```

---

### *Function:* GetControlLayout(Table) *returns* Table,Table

Define how the Controls are presented and behave on UI and Pins. This section represents the main chunk of work in the plugin presentation, and will be the location of most errors.

Returns two tables:

- **Layout** - Interactive controls
- **Graphics** - Aesthetic controls

---

#### Graphics Table: Images

Images can be included by encoding Svg, Png or Jpeg as a base64 string using a site such as:

- <https://www.base64-image.de/>
- <https://www.browserling.com/tools/image-to-base64>

For example of the encoded string provided, examine the raw source of this file. The image below is embeded as:

```markdown
data:image/png;base64,<base64EncodedTextHere>
```

and produces this image

![Hello World](data:image/png;base64,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)

Example:

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

---

#### Graphics Table: Text Field

Display static text

Example:

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

---

## NuSpec File Structure

### MetaData

#### Author

- "QSC" will cause plugin to appear in "QSC Managed" tree
- Anything else will appear in "User" tree

### Description

The description field is populated by the `description.md` file in the root of the plugin repo.

NuGet supports most simple markdown, and the deploy process will convert relative image links from markdown to the slightly different NuGet supported format so that you can create and preview the file in VS Code.

### Release Notes

To be included as part of the build at a later date, build up from Git Commit notes.

---
