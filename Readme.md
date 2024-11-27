MWE Code inspired by https://flyx.org/nix-flakes-latex/

By bisecting nixpkgs I found the first bad commit to be https://github.com/NixOS/nixpkgs/commit/7cd5f8cda00425cd4ed283642f9a297a17a8f6b6:
```
  7cd5f8cda00425cd4ed283642f9a297a17a8f6b6 is the first bad commit
  commit 7cd5f8cda00425cd4ed283642f9a297a17a8f6b6 (HEAD)
```

Unsurprisingly this was the commit that switched from texlive2023 to texlive2024...

After this commit I get (unless I provide -shell-escape to lualatex):
```
luaotfload | load : FATAL ERROR
luaotfload | load :   × Failed to load "fontloader" module "basics-gen".
luaotfload | load :   × Error message:
luaotfload | load :     × "...2024-texmfdist/tex/luatex/luaotfload/luaotfload-init.lua:301: system : no writeable cache path, quiting".
```
Also running "luaotfload --check-permission" before starting latexmk shows
```
luaotfload | diagnose : =============== file permissions ==============
luaotfload | diagnose : Checking permissions of .cache/texmf-var/luatex-cache/generic/.
luaotfload | diagnose : Owner: 1000, group 100, permissions rwxr-xr-x.
luaotfload | diagnose : Readable: ok.
luaotfload | diagnose : Writable: ok.
luaotfload | diagnose : Checking permissions of .cache/texmf-var/luatex-cache/generic/names.
luaotfload | diagnose : Owner: 1000, group 100, permissions rwxr-xr-x.
luaotfload | diagnose : Readable: ok.
luaotfload | diagnose : Writable: ok.
luaotfload | diagnose : Checking permissions of .cache/texmf-var/luatex-cache/generic/names/luaotfload-names.lua.gz.
```
