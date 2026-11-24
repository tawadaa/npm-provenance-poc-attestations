# npm-provenance-poc-attestations

Public home for Sigstore attestation **bundles** of the `zama-npm-provenance-poc` npm package. The source of the package is in a private repo (`zama-experimental/npm-provenance-poc`), so its attestations aren't retrievable via the GitHub API by outsiders — we publish them here as release assets instead.

## Verify a published version

```bash
PKG=zama-npm-provenance-poc VER=0.0.1
TGZ=$(npm pack "$PKG@$VER" | tail -n1)

gh release download "v$VER" \
  -R tawadaa/npm-provenance-poc-attestations \
  -p "${TGZ%.tgz}.sigstore.json"

gh attestation trusted-root > trusted_root.jsonl

gh attestation verify "$TGZ" \
  -R zama-experimental/npm-provenance-poc \
  --bundle "${TGZ%.tgz}.sigstore.json" \
  --custom-trusted-root trusted_root.jsonl
```

Exit 0 means GitHub witnessed that this exact `.tgz` was built by `publish.yml` in `zama-experimental/npm-provenance-poc`.
