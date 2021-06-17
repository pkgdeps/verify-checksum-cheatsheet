# verify-checksum-cheatsheet

Checksum CheatSheet.

You need to verify checksum before executing downloaded binary.


## shasum

[jq](https://github.com/stedolan/jq) is an example of `shasum`.

```bash
#!/bin/bash

# docs: https://github.com/pkgdeps/verify-checksum-cheatsheet
declare JQ_VERSION="1.6"
# Download binary and checksum
curl -sLO https://github.com/stedolan/jq/releases/download/jq-${JQ_VERSION}/jq-linux64 && \
curl -sL https://raw.githubusercontent.com/stedolan/jq/master/sig/v${JQ_VERSION}/sha256sum.txt -o jq.sha256sum
# Verify the checksum
grep -E "jq-linux64$" jq.sha256sum | shasum -a 256 -c - || (echo "Error: Not match jq SHA256." && exit 1)
# Add permission for executable and Rename to "jq"
chmod 755 jq-linux64 && ln -sfnv "$(pwd)/jq-linux64" "$(pwd)/jq"
```

- [pkgdeps/curl-jq-checksum-example](https://github.com/pkgdeps/curl-jq-checksum-example)
