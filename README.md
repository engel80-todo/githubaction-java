# githubaction

GCP, Docker, Terraform, Python, Sonarqube

[gke-workload-identity](https://github.com/DevSecOpsSamples/gke-workload-identity/blob/master/.github/workflows/build.yml)

gradle-cache 40s 
gradle-no-cache 44s

![build-time-vs](screenshots/build-time-vs.png?raw=true)

![github-cache](screenshots/github-cache.png?raw=true)

![github-cache-download](screenshots/github-cache-download.png?raw=true)


Run actions/cache@v3
Received 8388608 of 142609533 (5.9%), 8.0 MBs/sec
Received 125829120 of 142609533 (88.2%), 60.0 MBs/sec
Received 142609533 of 142609533 (100.0%), 60.7 MBs/sec
Cache Size: ~136 MB (142609533 B)
/usr/bin/tar -xf /home/runner/work/_temp/f9cf1074-a647-42aa-a006-05a83ef4b250/cache.tzst -P -C /home/runner/work/test-githubaction-java/test-githubaction-java --use-compress-program unzstd
Cache restored successfully
Cache restored from key: Linux-gradle-a8e851f2af992b0ae2f6269da8beb9ec5c2688713b5e69ad0df36425b639957e

## Reference

- https://github.com/actions/cache

- https://github.com/actions/cache/blob/main/examples.md#java---gradle