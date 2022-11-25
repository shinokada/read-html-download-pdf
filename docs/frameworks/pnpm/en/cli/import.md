
# pnpm import


`pnpm import` generates a `pnpm-lock.yaml` from another package manager's lockfile. Supported source files:
* `package-lock.json`
* `npm-shrinkwrap.json`
* `yarn.lock`

Note that if you have workspaces you wish to import dependencies for, they will need to be declared in a [pnpm-workspace.yaml](../pnpm-workspace_yaml.html) file beforehand.

<span style='float: footnote;'><a href="../index.html#toc">Go to TOC</a></span>
