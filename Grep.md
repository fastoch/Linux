# Introduction

The grep command is used to search a given file for lines containing a match for a particular word or a string.  

Its basic syntax is: `grep search_term filename`  

---

# Simple commands

- To ignore case on a grep command: `grep -i search_term filename`
- To search in multiple files: `grep search_term file1 file2`  

---

# More advanced commands

- to use regular expressions (regexp): `grep -e pattern filename`
- to use extended regexp: `grep -E pattern filename`  

>[!note]
>egrep and fgrep are deprecated commands since 2007: https://lists.gnu.org/archive/html/info-gnu/2022-09/msg00001.html

- to search recursively: `grep -r search_term directory_path` or `rgrep search_term directory_path`
- to select non-matching lines: `grep -v search_term filename`
- to print only a count of selected lines per file: `grep -c search_term filename`
- to print line numbers with output lines: `grep -n search_term filename`
- to grep against a file containing multiple patterns (one per line); `grep -f pattern_file filename`


---
EOF
