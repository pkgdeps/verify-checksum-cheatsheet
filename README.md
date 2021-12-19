# verify-checksum-cheatsheet

Checksum CheatSheet.

You need to verify checksum before executing downloaded binary.


## shasum

[jq](https://github.com/stedolan/jq) is an example of `shasum`.

masOS: use `shasum -a 256` instead of `sha256sum`

```bash
#!/bin/bash

# defined _sha256sum command
function _sha256sum {
  local cmd
  if command -v sha256sum &> /dev/null; then
    cmd=(sha256sum)
  elif command -v sha256 &> /dev/null; then
    cmd=(ssha256 -r)
  elif command -v shasum &> /dev/null; then
    cmd=(shasum -a 256)
  else
    echo "ERROR: could not find shasum or sha256sum."
    return 1
  fi

  "${cmd[@]}" "$@"
}
# docs: https://github.com/pkgdeps/verify-checksum-cheatsheet
declare JQ_VERSION="1.6"
# Download binary and checksum
curl -sLO https://github.com/stedolan/jq/releases/download/jq-${JQ_VERSION}/jq-linux64 && \
curl -sL https://raw.githubusercontent.com/stedolan/jq/master/sig/v${JQ_VERSION}/sha256sum.txt -o jq.sha256sum
# Verify the checksum
grep -e "jq-linux64$" jq.sha256sum | _sha256sum -c
# Add permission for executable and Rename to "jq"
chmod 755 jq-linux64 && ln -sfnv "$(pwd)/jq-linux64" "$(pwd)/jq"
```

Related examples:

- [pkgdeps/curl-jq-checksum-example](https://github.com/pkgdeps/curl-jq-checksum-example)
- [pkgdeps/curl-kustomize-checksum-example: Download kustomize command and verify the checksum using shasum.](https://github.com/pkgdeps/curl-kustomize-checksum-example)
