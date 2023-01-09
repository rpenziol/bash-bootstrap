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

```bash
# Restore from saved snapshot
# Note: the dataset "restore" doesn't have to exist before running this command
sudo zfs receive tank/restore < /backups/data.zfs
```

```bash
# Delete dataset and all its snapshots
sudo zfs destroy -r tank/test
```

## Git Rewrite History

```bash
# Install git-filter-repo
pip3 install git-filter-repo
```

```bash
# Rewrite author history - https://git-scm.com/docs/gitmailmap#_examples
origin=$(git remote get-url origin)

printf 'Robbie Penziol <rpenziol@users.noreply.github.com> <test@test.com>\nRobbie Penziol <rpenziol@users.noreply.github.com> <(none)>\nRobbie Penziol <rpenziol@users.noreply.github.com> <rpenziol@pdx.edu>\n' > ~/.mailmap
git filter-repo --mailmap ~/.mailmap --force

git remote add origin ${origin}
git push --set-upstream origin main --force
```

```bash
# Remove file/directory from history
origin=$(git remote get-url origin)

git filter-repo --path test/path --invert-paths --force
git filter-repo --path test_file.txt --invert-paths --force

git remote add origin ${origin}
git push --set-upstream origin main --force
```

```bash
# Rename branch from master -> main
git branch -m master main
git push -u origin main
# Manual step: swap default branch on Github from master to main
git push origin --delete master
```
