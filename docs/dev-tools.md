---
---

# Dev Tools

_Like [asdf](https://asdf-vm.com) (or [nvm](https://github.com/nvm-sh/nvm) or [pyenv](https://github.com/pyenv/pyenv) but for any language) it manages dev tools like node, python, cmake, terraform, and [hundreds more](/plugins)._

::: tip
New developer? Try reading the [Beginner's Guide](https://dev.to/jdxcode/beginners-guide-to-mise-ac4) for a gentler introduction.
:::

mise is a tool for managing programming language and tool versions. For example, use this to install
a particular version of Node.js and ruby for a project. Using `mise activate`, you can have your
shell automatically switch to the correct node and ruby versions when you `cd` into the project's
directory[^cd]. Other projects on your machine can use a different set of versions.

mise is inspired by [asdf](https://asdf-vm.com) and uses asdf's vast [plugin ecosystem](https://github.com/rtx-plugins/registry)
under the hood. However, it is _much_ faster than asdf and has a more friendly user experience.
For more on how mise compares to asdf, [see below](/comparison-to-asdf).

mise can be configured in many ways. The most typical is by `.mise.toml`, but it's also compatible
with asdf `.tool-versions` files. It can also use idiomatic version files like `.node-version` and
`.ruby-version`. See [Configuration](./configuration) for more.

[^cd]:
    Note that mise does not modify "cd". It actually runs every time the prompt is _displayed_.
    See the [What does `mise activate` do?](/faq#what-does-mise-activate-do).

* Like [direnv](https://github.com/direnv/direnv) it manages [environment variables](/configuration#env---arbitrary-environment-variables) for different project directories.
* Like [make](https://www.gnu.org/software/make/manual/make.html) it manages [tasks](/tasks/) used to build and test projects.

### How it works

mise hooks into your shell (with `mise activate zsh`) and sets the `PATH`
environment variable to point your shell to the correct runtime binaries. When you `cd` into a
directory[^cd] containing a `.tool-versions`/`.mise.toml` file, mise will automatically set the
appropriate tool versions in `PATH`.

After activating, every time your prompt displays it will call `mise hook-env` to fetch new
environment variables.
This should be very fast. It exits early if the directory wasn't changed or `.tool-versions`/`.mise.toml` files haven't been modified.

Unlike asdf which uses shim files to dynamically locate runtimes when they're called, mise modifies
`PATH` ahead of time so the runtimes are called directly. This is not only faster since it avoids
any overhead, but it also makes it so commands like `which node` work as expected. This also
means there isn't any need to run `asdf reshim` after installing new runtime binaries.

You should note that mise does not directly install these tools.
Instead, it leverages plugins to install runtimes.
See [plugins](/plugins) below.

[^cd]:
Note that mise does not modify "cd". It actually runs every time the prompt is _displayed_.
See the [FAQ](/faq#what-does-mise-activate-do).

### Common commands

```text
mise install node@20.0.0  Install a specific version number
mise install node@20      Install a fuzzy version number
mise use node@20          Use node-20.x in current project
mise use -g node@20       Use node-20.x as global default

mise install node         Install the current version specified in .tool-versions/.mise.toml
mise use node@latest      Use latest node in current directory
mise use -g node@system   Use system node as global default

mise x node@20 -- node app.js  Run `node app.js` node-20.x on PATH
```
