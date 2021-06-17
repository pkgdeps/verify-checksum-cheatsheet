# verify-checksum-cheatsheet

Checksum CheatSheet.

You need to verify checksum before executing downloaded binary.


## shasum

[jq](https://github.com/stedolan/jq) is an example of `shasum`.

```bash
#!/bin/bash

declare JQ_VERSION="1.6"
# Download binary and checksum
# → Download these ./jq-linux and ./jq.sha256sum
# Note: Preserve binary file name because shasum command read the file name like "jq-linux64"
curl -sLO https://github.com/stedolan/jq/releases/download/jq-${JQ_VERSION}/jq-linux64 && \
curl -sL https://raw.githubusercontent.com/stedolan/jq/master/sig/v${JQ_VERSION}/sha256sum.txt -o jq.sha256sum
# Verify the checksum
# → read jq-linux64 line from and pass to shasum command
# → If the checksum of jq-linux64 is not same to jq.sha256sum's value, show error and exit 1
grep -E "jq-linux64$" jq.sha256sum | shasum -a 256 -c - || (echo "Error: Not match jq SHA256." && exit 1)
# Add permission for executable and Rename to "jq"
# → You can use "jq" command
chmod 755 jq-linux64 && ln -sfnv "$(pwd)/jq-linux64" "$(pwd)/jq"
```

- [pkgdeps/curl-jq-checksum-example](https://github.com/pkgdeps/curl-jq-checksum-example)
