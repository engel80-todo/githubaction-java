# githubaction

## Build cache for Gradle

[.github/workflows/build-java.yml](.github/workflows/build-java.yml):

```bash
name: Build
on:
  push:
    branches:
      - master
      - develop
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  gradle-cache:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: 11
      - name: Cache Gradle packages
        uses: actions/cache@v3
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper
          key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle', '**/gradle-wrapper.properties') }}
          restore-keys: ${{ runner.os }}-gradle
      - name: Build and analyze
        run: ./gradlew build --info
          
  gradle-no-cache:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: 11
      - name: Build and analyze
        run: ./gradlew build --info
```
### Time Taken

| Action Step            | Cache Miss  | Cache Hit  |
|------------------------|-------------|------------|
| Cache Gradle packages  | 0s          | 8s   |
| Grable build           | 33s         | 8s   |

- Cache miss:

    ![cache-miss.png](screenshots/cache-miss.png?raw=true)

    Download gradle wrapper and SpringBoot dependencies.

- Cache hit:

    ![cache-hit.png](screenshots/cache-hit.png?raw=true)

    ![github-cache](screenshots/github-cache.png?raw=true)

## Reference

- https://github.com/actions/cache

- https://github.com/actions/cache/blob/main/examples.md#java---gradle
