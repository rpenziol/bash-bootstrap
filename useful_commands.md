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
