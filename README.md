# Optimize Gradle build speed using cache plugin

## GitHub Workflow

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

[.github/workflows/build-java-sonarqube.yml](.github/workflows/build-java-sonarqube.yml):

### Time Taken

| Action Step            | Cache Miss  | Cache Hit  |
|------------------------|-------------|------------|
| Set up JDK 11          | 6s          | 6s   |
| Cache Gradle packages  | 0s          | 8s   |
| Grable build           | 33s         | 8s   |

You can speed up build time 17 seconds.

- Cache miss:

    ![cache-miss](screenshots/cache-miss.png?raw=true)

    Download gradle wrapper and SpringBoot dependencies.

- Cache hit:

    ![cache-hit](screenshots/cache-hit.png?raw=true)

    ![github-cache](screenshots/github-cache.png?raw=true)

Run details in usage menu:

![build-time-in-usage](screenshots/build-time-in-usage.png?raw=true)


## Reference

- [GitHub Actions /Using workflows / Cache dependencies / Caching dependencies to speed up workflows](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#managing-caches)

- https://github.com/actions/cache

- https://github.com/actions/cache/blob/main/examples.md#java---gradle
