
### count regex matches grouped by exact value
```bash
grep  -o -e '<regex>' | sort | uniq -c
```

### largest directories under current directory
```bash
du -sh * | sort -rh
```

### split/cut lines of file
```bash
cut -d<delim> -f<1BasedIndexOfField> <filename>
```
