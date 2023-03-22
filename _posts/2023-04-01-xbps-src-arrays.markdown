---
title: Major changes to xbps-src template format
layout: post
---

[`xbps-src` package templates](https://github.com/void-linux/void-packages) now require many variables to be formatted
as [bash arrays](https://www.gnu.org/software/bash/manual/html_node/Arrays.html).
This change allows templates to be more flexible (like being able to quote arguments passed to build commands)
and allows some internal `xbps-src` changes to improve functionality.

The new rules are as follows:

1. If a variable has one value, it can remain as a string, and xbps-src will internally convert it to a single-element array:

```bash
configure_args="--enable-foo"
```

2. If a variable has multiple values, it must be an array:

```bash
configure_args=(--enable-foo --disable-bar --with-libbaz="${XBPS_CROSS_BASE}/usr/include/baz")
```

3. If needed, arguments can be quoted within the array, and will not be separated when passed to other commands:

```bash
configure_args=("-DFOO=bar baz" "-DBAR=baz" '-DBAZ=blah')
```

`xbps-src` will error out if a variable looks like it should be converted from a string to an array
(i.e., if it has spaces in it). If it should have multiple items, convert it to an array.
If it should still be one string, wrap the string in `()` so that it is a single-element array.

The following variables must now be arrays:

- `hostmakedepends`, `makedepends`, `checkdepends`, and `depends`
- `configure_args`, `make_build_args`, `make_install_args`, `make_check_args`
- `make_build_target`, `make_install_target`, `make_check_target`
- `configure_script`, `make_cmd`, `make_check_pre`
- `archs`
- `build_helper`
- `distfiles`
- `checksum`
- `skip_extraction`
- `subpackages`
- `reverts`
- `conflicts`
- `replaces`
- `alternatives`
- `shlib_provides`, `shlib_requires`
- `nopie_files`, `nostrip_files`, `conf_files`, `mutable_files`
- `ignore_elf_files`, `ignore_elf_dirs`
- `make_dirs`
- `perl_configure_dirs`
- `font_dirs`
- `dkms_modules`
- `register_shell`
- `tags`
