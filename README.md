# avast.lua

Copyright © 2020 Ralph Seichter. Sponsored by [sys4 AG](https://sys4.de/).

## Description

[avast.lua](https://github.com/rseichter/rspamd-avast) is a [Rspamd](https://www.rspamd.com/) antivirus extension
module for the [Avast](https://www.avast.com/de-de/index#mac) virus scanner, written in [Lua](https://www.lua.org).
The module was tested with Rspamd 2.2 and Avast 3.0.3.

## License

Apache License 2.0, see license file for [details](LICENSE).

## Prerequisites

avast.lua requires [lua-socket](http://w3.impa.br/~diego/software/luasocket/home.html) version 3. For Debian 10 you
can use `apt install lua-socket` to install the necessary package. If this C-module is _not_ installed in Rspamd's
own library path, you have to manually set the `cpath_prefix` parameter or avast.lua won't work. See Lua's
[package.loaders](https://www.lua.org/manual/5.1/manual.html#pdf-package.loaders) for a detailed syntax description.

As of January 2020, Avast only supports UNIX Domain Sockets, so the virus scanner must run on the same machine
as Rspamd. Please ensure that the domain socket allows R/W access for the Rspamd process and that you set the
socket path in your `antivirus.conf` (the default value is `/run/avast/scan.sock`). The `tmpdir` parameter defaults
to the value of the TMPDIR environment variable (if available) or `/tmp`.

## Installation

*  Place a copy of `avast.lua` in your `/usr/share/rspamd/lualib/lua_scanners` directory.
*  Add the line `require_scanner('avast')` to `lua_scanners/init.lua`.
*  Add a section to your `local.d/antivirus.conf`. Adapt parameters to your local environment, e.g. by changing
the values for cpath_prefix and socket, if necessary.
```
avast {
  type = 'avast';
  # Lua package.cpath prefix (example for Debian 10). 
  #cpath_prefix = '/usr/lib/x86_64-linux-gnu/lua/5.1/?.so';
  # Avast socket path
  #socket = '/run/avast/scan.sock';
  # Message content is temporarily stored in this directory
  #tmpdir = '/tmp';
  # Log clean files as well
  #log_clean = false;
  # Force this action if any virus is found (default: unset)
  action = 'reject';
}
```
*  Restart Rspamd.
