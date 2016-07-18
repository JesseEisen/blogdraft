---
title: Shell Script Practical
updated: 2016-07-18 20:00
---

## Printf better than echo
https://unix.stackexchange.com/questions/65803/why-is-printf-better-than-echo

## Ask first
https://www.reddit.com/r/commandline/comments/4pdghh/simple_trick_use_echo_to_make_sure_youre_deleting/

有时候在执行一些命令的时候，我们习惯性使用通配符来批量操作。在bash中，对于一些没办法挽回的操作，我们需要很小心。最简单的方式是在你执行的命令前加上`echo`. 比如：


```
echo rm -rf *.txt
```

## Error Handle
http://fvue.nl/wiki/Bash:_Error_handling

## set -u or -e 
http://mywiki.wooledge.org/BashFAQ/112