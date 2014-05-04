duke-of-url
===========

Predicts next URLs from browsing history using NuPIC.

# Running
1. Extract the dataset into a file under this repo called data/raw.csv, as described below
1. Sanitize the data by running`python py/sanitize.py`
1. Run a swarm over the dataset using data/sanitized.csv as the datafile and search_def.json as the config file

# Dataset

## Chrome on Mac

Export chrome history into pipe-delimited data file called raw.csv

```
/usr/bin/sqlite3 ~/Library/Application\ Support/Google/Chrome/Default/History > data/raw.csv <<EOF
SELECT replace(urls.url, '|', 'b'), urls.visit_count, urls.typed_count, datetime((urls.last_visit_time/1000000)-11644473600, 'unixepoch', 'localtime'), urls.hidden, datetime((visits.visit_time/1000000)-11644473600, 'unixepoch', 'localtime') as visittime, visits.from_visit, visits.transition
FROM urls, visits
WHERE urls.id = visits.url
order by visittime asc;
EOF
```

If you're curious what's in the URL table, try this.

```
/usr/bin/sqlite3 ~/Library/Application\ Support/Google/Chrome/Default/History
> PRAGMA table_info(urls);
```

## TLDs

Original source:
https://mxr.mozilla.org/mozilla/source/netwerk/dns/src/effective_tld_names.dat?raw=1