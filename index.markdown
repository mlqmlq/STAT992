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
Notice that there is a `journalName` column in the JSON file, we can try to extract the journalName and check whether the paper was pulbished in one of those four journals. In terms of efficiency, we wrote a shell script utilizing text stream. We used the `jq` command for extracting information from the JSON file.

Firstly, we give a brief tutorial of the linux utility `jq`.

[jq](https://stedolan.github.io/jq/) is a light weight and flexible command-line JSON processor. We can use it to slice and filter and map and transform structured data with the same ease as `sed`, `awk`, `grep`. `jq` is written in portable C, and it has zero runtime dependencies. We can download a single binary, `scp` it to a far away machine of the same type, and expect it to work.

### Install `jq`
If you are on a Mac, you can simply install it using `homebrew`. 
```sh
brew install jq
```
If you are on Linux, you can utilize `apt-get`.
```sh
sudo apt-get install jq
```
The `.{attribute name}` syntax can be used to extract the corresponding attribute.

Specifically, in our problem,
```shell
jq '.journalName'
```
will return the journal name of each line. The entire shell script we wrote is as follows:

```shell
#!/bin/bash
str1="\"Journal of the American Statistical Association\""
str2="\"Annals of Statistics\""
str3="\"Biometrika\""
str4="\"Journal of The Royal Statistical Society Series B-statistical Methodology\""
while read p; do
  k=$(echo "$p" | jq '.journalName')
  echo "$k"
  if [[ "$k" == "$str1" || "$k" == "$str2" || "$k" == "$str3" || "$k" == "$str4" ]]
  then
    echo "$p" >> out.json
    continue
  fi
done < s2-corpus-000 

```
In this way, all of the entries satisfying our condition will be written to a file called `out.json`. Then, we can do the subsequent analysis using Python/R.





