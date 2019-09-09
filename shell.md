# Bash Commands

## 1. count regex matches grouped by exact value

```bash
grep  -o -e '<regex>' | sort | uniq -c
```

## 2. largest directories under current directory

```bash
du -sh * | sort -rh
```

## 3. split/cut lines of file

```bash
cut -d<delim> -f<1BasedIndexOfField> <filename>
```

## 4. wait for process to be done

```bash
tail --pid="${PID}" -f /dev/null
```
