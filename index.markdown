---
title: [STAT 992]
layout: home
---

# STAT 992: Data Science with Graphs

## Team Member (alphabetical order): Yutian Liu, Ziyi Liu, Linquan Ma, Peng Yu 

## Blog post: Data Preprocessing
In the final project, we plan to study the authors who published papers in top statistical journals (JASA, JASSB, AOS, Biometrika). In this post, we illustrate how we did data preprocessing.

After downloading the data, we have in total 186 zipped files with each around 1.5 GB after unzipping. The first line of the first JSON file has the following form:

```JSON
{"entities":[],"magId":"409509287","journalVolume":"37","journalPages":"141-145","pmid":"","fieldsOfStudy":["Environmental Science"],"year":1993,"outCitations":[],"s2Url":"https://semanticscholar.org/paper/5bd3e1fe913dea18eb5d485b3c481e491d3afd04","s2PdfUrl":"",
"id":"5bd3e1fe913dea18eb5d485b3c481e491d3afd04","authors":[{"name":"佐藤 正仁","ids":["87354532"]},{"name":"大野 芳和","ids":["108325260"]}],"journalName":"","paperAbstract":"","inCitations":[],"pdfUrls":[],"title":"熱帯地域における農業振興と環境保全 Ｉｖ．農業生産性向上と環境保全 熱帯畑作と環境保全","doi":"","sources":[],"doiUrl":"","venue":""}

```
Notice that there is a `journalName` column in the JSON file, we can try to extract the journalName and check whether the paper was pulbished in those four journals or not. In terms of efficiency, we wrote a shell script using text stream. We used the `jq` command for extracting information from the JSON file.

Firstly, we give a brief tutorial of the linux utility `jq`.

Specifically, 
```shell
jq '.fieldsOfStudy|.[0]'
```
will return the field name of each line. The entire shell script is as follows:

```shell
#!/bin/bash
str1="\"Mathematics\""
str2="\"Computer Science\""
while read p; do
  k=$(echo "$p" | jq '.fieldsOfStudy|.[0]')
  if [[ "$k" == "$str1" || "$k" == "$str2" ]]
  then
    echo "$p" >> out.json
    continue
  fi
done <s2-corpus-000 

```
We iterate through each line and check whether the `fieldOfStudy` equals to `Mathematics` or `Computer Science`. If so, we copy the line to the file `out.json`.






