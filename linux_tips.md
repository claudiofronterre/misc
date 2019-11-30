# Useful tips for Linux

## Search and find all files for a given text string

With the `grep` command we can look for a specific text strng in a file. The `-r` option will look recursively in all the folders inside `directory-path` (and in all the files inside those folders). By default, the `grep` command prints the matching lines. With the `-H` option it will print only the filename for each match:

```
grep -r -H "text string to search" directory-path
```
