---
layout: post
title: "Kibana vs. Dashboards vs. UUID"
categories: tips, kibana, dashboard, dfir
date: "05-04-2021"
---

## [Context](#context)

I was recently trying to add a few security oriented dashboards to Kibana.
I was fortunate enough to find the dashboards online in JSON form, so all I needed to do was feed them into Kibana, and be done with it, right?

## [Difficulties](#difficulties)

Well, no, not quite. The ELK stack I was working with had logs from a production environment and a staging environment, and I needed a set of dashboards for each.

So I ran a search/replace on the JSON I had to add a prefix (either `prod` or `dev`) to each dashboard or visualization in the file. I uploaded the 2 resulting files to Kibana, only to find out that only one set of dashboards remained after the fact.

Long story short, Kibana was overwriting one set with the other because I had not changed the UUIDs in the original JSON file. Silly me, thinking that Kibana would handle its own UUIDs behind the scenes without bothering me.

## [Solution](#solution)

So I put together a quick bash script to replace all occurrences of a single UUID with a new random UUID.

```bash
#/bin/bash

INPUT_DASHBOARD_JSON=dashboards.json
OUTPUT_DASHBOARD_JSON=result.json

cp $INPUT_DASHBOARD_JSON tmpfile

# Select the right columns to cut to get only the UUID
grep '"_id": ' $INPUT_DASHBOARD_JSON | cut -c13-48 > tmp_ids 

while read p; do
  uuid=$(echo $p | tr -d '\n')
  echo "$uuid"
  sed -i "s/$uuid/$(uuidgen)/g" tmpfile
done <tmp_ids

rm tmp_ids
mv tmpfile $OUTPUT_DASHBOARD_JSON
```

`sed` and `uuidgen` are the real heroes here.
