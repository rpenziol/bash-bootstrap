# Dot Files and Handy Documentation

## Setup 1-liner
```bash
dst="/tmp/dotfiles" ; mkdir -p ${dst} ; git clone git@github.com:rpenziol/bash-bootstrap.git ${dst} ; cp -rT ${dst} ~/ --backup=numbered ; rm -rf ${dst}
```

## Purge all old versions of files
```bash
find ~/ -maxdepth 1 -regex '.*~[0-9]+~' -exec rm "{}" \;
```
