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

This proposal aims to introduce a new conventional place for storing configuration files and motivate different tools to support it as a new alternative standard while allowing top-level conventions the same as before.

## `.config/` directory

When the `.config/` directory exists, tools read the config files inside this directory.

Usually, tools should use `<dir>/.config/[name].[ext]` for file name convention.

### Nesting

As the size of the configuration increases, managing them all in a single configuration will be harder. Tools can optionally support `.config/[name]/` to allow nesting.

In the case of monorepo when users need to specify multiple files of the same config, the config files can be nested into `/<path>/.config` directory and based on the tool requirements either merged with mono repo's `/.config` or not.

### Conventions

While this proposal does not enforce the naming convention of files inside this dir, it provides some  recommendations and best practices.


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

Please see [this discussion](https://github.com/pi0/config-dir/discussions/3).

## Inspirations

Please see [this discussion](https://github.com/pi0/config-dir/discussions/5).

## Current efforts

### `unjs/c12`

[unjs/c12](https://github.com/unjs/c12) is a configuration loader for JavaScript used by [Nuxt](https://nuxt.com/), [Nitro](https://nitro.unjs.io/) and [more](https://github.com/unjs/c12?tab=readme-ov-file#-used-by) projects.

Config directory support is planned as an opt-in feature ([tracker issue](https://github.com/unjs/c12/issues/134)).

## Next steps

This is a big ecosystem issue and unless we push to unblock it, nothing will happen! The goal of this proposal is to make a centralized effort and discussion forum for tool authors to contribute.

- Make a tracker DB of tools with known configuration files and their status with this proposal
- Open individual issue trackers to each tool and invite them to collaborate
- Provide a preset configuration to use VSCode File Nesting with the `.config/` directory

