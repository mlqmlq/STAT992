---
title: [STAT 992]
layout: home
---

## STAT 992: Data Science with Graphs

### Team Member (alphabetical order): Yutian Liu, Ziyi Liu, Linquan Ma, Peng Yu 

### Blog post: Data Preprocessing

```shell
#!/bin/bash
str1="\"Mathematics\""
str2="\"Computer Science\""
while read p; do
  k=$(echo "$p" | jq '.fieldsOfStudy|.[0]')
  if [[ "$k" == "$str1" || "$k" == "$str2" ]]
  then
    echo "$k"
    echo "$p" >> out.json
    continue
  fi
done <s2-corpus-000 

```
