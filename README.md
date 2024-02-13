# 📁 config directory proposal

A proposal for the `.config/` directory.

## Motivation

The number of configs in projects is growing every day, and there is no convention to organize them!

The lack of a conventional place to organize configuration files led tools to often prefer to use top-level configuration files suffixed with `.config` or `.rc`.

This makes things harder than they should be:

- For developers to quickly get started and understand a new codebase
- For repository maintainers, to manage config files
- For tools to distinguish config files
- For library authors to decide on where to store or load configs

## Goal

This proposal aims to introduce a new conventional place for storing configuration files and **encourage** tool authors to support it.

Migrating to a new convention is understandably a complex process. This proposal encourages to use of the config dir convention as an **alternative** method while allowing top-level conventions.

Tool authors shall decide to encourage the users to use an organized configuration directory and prefer it as the canonical place.

## `.config/` directory

When the `.config/` directory exists, tools shall respect the config files inside it and use them.

Tools should use `<dir>/.config/[name].[ext]` for their

### Nesting

As the number of configuration options increases, managing them all in a single configuration will be impossible.

## Conventions

While this proposal does not enforce the naming convention of files inside this dir, this proposal provides some conventional recommendations and best practices.

### Specify tool name

Tool or framework names should be explicitly clear in configuration file names to avoid conflicting tools with each other and also make it clear for users what config is for what.

✅ `.config/toolname.js`
❌ `.config/app.js`

### Avoid the `.config` and `.conf` suffix

Since the `.config` directory name is already clear it is holding configuration files, the `.config` suffix is not needed and shall be avoided

> [!NOTE]
> To make the migration process easier for end-users (ie: drag and drop from the top level to the `.config` directory), tools shall allow this suffix but also encourage users to not use it.

✅ `.config/toolname.js`
⚠️ `.config/toolconf.js`
⚠️ `.config/toolname.config.js`

### Specify format with extension

Config files without clear extensions are harder to be parsed. Both by IDEs and other tools and also for end-users to understand.

> [!NOTE]
> We discourage using the `rc` format because the interpretation is ambiguous.

✅ `.config/toolname.toml`
❌ `.config/toolname`
❌ `.config/toolrc`

### Lower-case

Using case-sensitive configuration file names can introduce cross-platform support issues. It is highly recommended to only use lowercase naming.

> [!NOTE]
> This proposal recommends using the kebab case if the tool name is too long `tool-name.json` instead of a camel case like `tool_name.json` to preserve consistency in this directory.

✅ `.config/toolname.toml`
❌ `.config/toolName`


## Alternative directory names

Before writing this proposal I had been thinking of `.conf/` as a shorter alternative. it also has fewer chances of existing usage tools that consume the `.config` directory. As a drawback, this name is less readable. Also, we have existing conventions such as [XDG](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) that prefer `.config`.

## `.env` files

This proposal currently has no recommendations on how to handle `.env` files.

## VSCode file nesting

Visual Studio Code, [introduced]((https://code.visualstudio.com/updates/v1_64#_explorer-file-nesting)) a new experimental feature called "file nesting" that allows displaying the same directory in a logically nested layout. Using a preset like [`antfu/vscode-file-nesting-config`](https://github.com/antfu/vscode-file-nesting-config) it is possible to virtually organize config files into an alternative structure.

While this feature can serve as a good workaround for the current situation, it doesn't solve the real problem. Actual files are still stored in the top-level directory in an unorganized manner. When browsing project code in other tools, like File Explorer, Github, or Gitlab, they are still not nested.

## Previous efforts

### Tweets

I was inspired by seeing [tweet](https://twitter.com/DanaWoodman/status/1699134345196495182) by Dana Woodman about this problem a few months ago and while he was [convienced](https://twitter.com/DanaWoodman/status/1699535674867949905) to use file nesting, the real problem still exists with actual files not being organized!

## Current efforts

### `unjs/c12`

[unjs/c12](https://github.com/unjs/c12) is a configuration loader for JavaScript used by [Nuxt](https://nuxt.com/), [Nitro](https://nitro.unjs.io/) and [more](https://github.com/unjs/c12?tab=readme-ov-file#-used-by) projects.

Config directory support is planned as an opt-in feature ([tracker issue](https://github.com/unjs/c12/issues/134)).

## Next steps

This is a big ecosystem issue and unless we push to unblock it, nothing will happen! The goal of this proposal is to make a centralized effort and discussion forum for tool authors to contribute.

- Make a tracker DB of tools with known configuration files and their status with this proposal
- Open individual issue trackers to each tool and invite them to collaborate
- Provide a preset configuration to use VSCode File Nesting with the `.config/` directory

