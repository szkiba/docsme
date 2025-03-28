# ｄｏｃｓｍｅ

**Keep README up to date based on CLI help**

The **docsme** go library makes it possible to automatically keep the documentation of command line tools up-to-date based on their CLI help.  Markdown documentation can be generated from [Cobra](https://github.com/spf13/cobra) command definitions. The generated documentation can be used to update part of `README.md` or can be written to a separate file.

### Features

- documentation and CLI help are guaranteed to be in sync
- documentation can be kept up to date automatically
- used only at build time, does not increase the size of the CLI tool
- gives motivation to write good CLI help

### Usage

**As a build-time tool.**

The advantage of using **docsme** as a build-time tool is that the **docsme** library is not built into the CLI at all.

To use it as a build-time tool, a `tools/docsme/main.go` file should be created with content similar to the following:

```go
package main

import (
    "github.com/spf13/cobra"
    "github.com/szkiba/docsme"
)

// newCommand is a factory function for generating the root cobra.Command.
func newCommand() *cobra.Command {
    root := &cobra.Command {
        // command definition goes here
    }

    // add subcommands here

    return root
}

func main() {
    cobra.CheckErr(docsme.For(newCommand()).Execute())
}
```

Help is available with the usual `--help` flag.

```bash
go run ./tools/docsme --help
```

**As a subcommand**

The advantage of using **docsme** as a subcommand is that documentation can be generated at any time with the CLI tool. Since **docsme** is a small library without external dependencies, embedding it does not increase the size of the CLI.

To use it as a subcommand, the **docsme** Cobra command definition should be added to the CLI command.

```go

package main

import (
    "github.com/spf13/cobra"
    "github.com/szkiba/docsme"
)

// newCommand is a factory function for generating the root cobra.Command.
func newCommand() *cobra.Command {
    root := &cobra.Command {
        Use: "mycli",
        // command definition goes here
    }

    root.AddCommand(docsme.New())

    return root
}

func main() {
    cobra.CheckErr(newCommand().Execute())
}
```

The `docsme.New()` factory function creates the Cobra command definition with the name `docs`. Help is available from the `docs` subcommand with the `--help` flag.

```bash
mycli docs --help
```

### Status

Following [Readme Driven Development](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html), the README of the **docsme** library was created first. Implementation then began on `2025-03-28`.

The initial implementation is currently being developed on the [`1-initial-implementation`](https://github.com/szkiba/docsme/tree/1-initial-implementation) branch.
