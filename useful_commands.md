# Useful Commands

## Redirect

```bash
# STDOUT to STDERR
2>&1
```

```bash
# Direct output to STDOUT and a file
sample command | tee ${FILENAME}
```

## 7z

```bash
# Compress all folders in current path
find . -type d -print0 | sort -z | xargs -0 -I{} sh -c 'dir=$(dirname -- "{}") ; filename=$(basename -- "{}") ; 7z a -t7z -m0=lzma -mx=9 -mfb=64 -md=32m -ms=on "${dir}/${filename%.*}.7z" "{}"'
```

## Zip

```bash
# Zip all files with given extension to zip file with only the single file inside the archive
find . -type f -regex ".*\.\(txt\|log)" -print0 | sort -z | xargs -0 -I{} sh -c 'dir=$(dirname -- "{}") ; filename=$(basename -- "{}") ; zip --junk-paths "${dir}/${filename%.*}.zip" "{}"'
```

## USB devices

```bash
dmesg
```

## ZFS

```bash
sudo zpool scrub storage
```

```bash
zpool status
```

```bash
# List all zpools and datasets
zfs list
```

```bash
# Delete all snapshots
zfs list -H -o name -t snapshot | xargs -n1 sudo zfs destroy
```

## Git Rewrite History

```bash
# Install git-filter-repo
pip3 install git-filter-repo
```

```bash
# Rewrite author history - https://git-scm.com/docs/gitmailmap#_examples
printf 'Robbie Penziol <rpenziol@users.noreply.github.com> <test@test.com>\nRobbie Penziol <rpenziol@users.noreply.github.com> <(none)>\n' > ~/.mailmap
git filter-repo --mailmap ~/.mailmap --force
```

```bash
# Remove file/directory from history
git filter-repo --path test/path --invert-paths --force
git filter-repo --path test_file.txt --invert-paths --force
```
