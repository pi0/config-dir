# 📁 config directory proposal

A proposal for the `.config/` directory.

## Motivation

The number of configs in projects is growing every day, and there is no convention to organize them!

The lack of a conventional place to organize configuration files led tools to often prefer to use top-level configuration files suffixed with `.config` or `rc`.

This makes things harder than they should be:

- For developers to quickly get started and understand a new codebase
- For repository maintainers, to manage config files
- For tools to distinguish config files
- For library authors to decide on where to store or load configs

## Goal

This proposal aims to introduce a new conventional place for storing configuration files and motivate different tools to support it as a new alternative standard while allowing top-level conventions same as before.

## `.config/` directory

When the `.config/` directory exists, tools read the config files inside this directory.

### Nesting

As the number of configuration options increases, managing them all in a single configuration will be impossible. Tools can optionally support `.config/[name]/` to allow nesting.

### Conventions

While this proposal does not enforce the naming convention of files inside this dir, it provides some  recommendations and best practices.

Usually, tools should use `<dir>/.config/[name].[ext]` for file name convention.

#### Specify tool name

Tool or framework names should be explicitly clear in configuration file names to avoid conflicting tools with each other and also make it clear for users what config is for what.

```
✅ .config/toolname.js

❌ .config/app.js
```

#### Avoid the `.config` and `.conf` suffix

Since the `.config` directory name is already clear it is holding configuration files, the `.config` suffix is not needed and shall be avoided.

```
✅ .config/toolname.js

❌ .config/toolconf.js
❌ .config/toolname.config.js
```

> [!NOTE]
> To make the migration to the `.config` directory process easier and keep the existing file name conventions, tools might allow this suffix as an alternative.

#### Specify format with extension

Config files without clear extensions are harder to be parsed. Both by IDEs and other tools and also for end-users to understand.

```
✅ .config/toolname.toml

❌ .config/toolname
❌ .config/toolrc
```

> [!Note]
> This proposal discourages using the `rc` format because the syntax interpretation is ambiguous.

#### Lower-case

Using case-sensitive configuration file names can introduce cross-platform support issues. It is highly recommended to only use lowercase naming.

```
✅ .config/toolname.toml

❌ .config/toolName
```

> [!NOTE]
> This proposal recommends using the kebab case if the tool name is too long `tool-name.json` instead of a camel case like `tool_name.json` to preserve consistency in this directory.

## Exceptional files

There are config/dot files that are common and unlikely to be nestable. These are excluded from the scope of this proposal:

- `.env`
- `.gitignore`

## Alternative directory names

Here are some alternative ideas for the config directory name collected from several places.

### `.config/` (current proposal)

Config directory indicates it holds the configuration files by their name and is a refactored part of the common `.config` suffix in current config file names.

It is prefixed with a dot `.` so that it will be sorted as the first directory and more clearly separated from the rest of the project, reducing more noise. Also is consistent with [XDG](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)'s `~/.config` directory.

### `.conf/`

Before writing this proposal I had been thinking of `.conf/` as a shorter alternative. As a drawback, this name is less readable.

### `.meta/`

[@barelyreaper](https://twitter.com/barelyreaper) [suggested](https://twitter.com/barelyreaper/status/1757385448266355025) to use `.meta/` since most of the (current) config files have `.config` in their name and with a more generic place like `.meta/` we can put other kinds of dotfiles such as `prettierignore` in it.

### `config/`

[@nainemom](https://github.com/nainemom) [suggested](https://github.com/pi0/config-dir/issues/1) to use the `config` directory instead of `.config` so that it won't be hidden in Unix tools.

## VSCode file nesting

Visual Studio Code, [introduced]((https://code.visualstudio.com/updates/v1_64#_explorer-file-nesting)) a new experimental feature called "file nesting" that allows displaying the same directory in a logically nested layout. Using a preset like [`antfu/vscode-file-nesting-config`](https://github.com/antfu/vscode-file-nesting-config) it is possible to virtually organize config files into an alternative structure.

While this feature can serve as a good workaround for the current situation, it doesn't solve the real problem. Actual files are still stored in the top-level directory in an unorganized manner. When browsing project code in other tools, like File Explorer, Github, or Gitlab, they are still not nested.

## Inspirations

I was inspired by seeing [tweet](https://twitter.com/DanaWoodman/status/1699134345196495182) by Dana Woodman about this problem a few months ago and while he was [convienced](https://twitter.com/DanaWoodman/status/1699535674867949905) to use file nesting, the real problem still exists with actual files not being organized!

[cosmiconfig](https://github.com/cosmiconfig/cosmiconfig) is a configuration loader for JavaScript that supports `.config/[name].[ext]` out of the box. (Thanks to @[KubaJastrz](https://github.com/KubaJastrz) for reference to it).

## Current efforts

### `unjs/c12`

[unjs/c12](https://github.com/unjs/c12) is a configuration loader for JavaScript used by [Nuxt](https://nuxt.com/), [Nitro](https://nitro.unjs.io/) and [more](https://github.com/unjs/c12?tab=readme-ov-file#-used-by) projects.

Config directory support is planned as an opt-in feature ([tracker issue](https://github.com/unjs/c12/issues/134)).

## Next steps

This is a big ecosystem issue and unless we push to unblock it, nothing will happen! The goal of this proposal is to make a centralized effort and discussion forum for tool authors to contribute.

- Make a tracker DB of tools with known configuration files and their status with this proposal
- Open individual issue trackers to each tool and invite them to collaborate
- Provide a preset configuration to use VSCode File Nesting with the `.config/` directory

