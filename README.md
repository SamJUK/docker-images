# docker-images

Container images I build for my own stacks and demo stores, published to GHCR. Each lives in `images/<name>` with a
`Dockerfile` and a `test.sh`, and has its own workflow in `.github/workflows/<name>.yml`. Pull requests build and test;
master publishes. Pin images by digest.

| Image | What it is |
|---|---|
| [`ghcr.io/samjuk/opensearch-slim`](#opensearch-slim) | OpenSearch 3.9.0 without its bundled plugins, for Magento |

## opensearch-slim

`ghcr.io/samjuk/opensearch-slim:3.9.0` (amd64 and arm64) is OpenSearch 3.9.0 with all 26 bundled plugins removed.
Magento's catalogue search needs only the core modules (`analysis-common`, `lang-painless`). It also runs on a
read-only root filesystem with a `noexec` tmpfs on `/tmp`.

Heap and JVM flags stay in `OPENSEARCH_JAVA_OPTS`. For a small demo catalogue, this held idle memory at about
320 MiB, against about 1 GiB for the stock image with a 512m heap:

```
-Xms64m -Xmx256m -XX:-AlwaysPreTouch -XX:-UseG1GC -XX:+UseSerialGC -XX:TieredStopAtLevel=1
```

Test it locally:

```sh
docker build -t opensearch-slim:test images/opensearch-slim
sh images/opensearch-slim/test.sh opensearch-slim:test
```

## Licence

The build files here are MIT. Each image keeps its upstream's licence (OpenSearch is Apache-2.0).
