
# pnpm unlink


Unlinks a system-wide package (inverse of [`pnpm link`](./link.html)

If called without arguments, all linked dependencies will be unlinked.

This is similar to `yarn unlink`, except pnpm re-installs the dependency after
removing the external link.

## Options

### --recursive, -r

Unlink in every package found in subdirectories or in every workspace package,
when executed inside a [workspace](../workspaces.html)

### --filter &lt;package_selector\>

[Read more about filtering.](../filtering.html)
<span style='float: footnote;'><a href="../index.html#toc">Go to TOC</a></span>
