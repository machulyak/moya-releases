# MOYA Releases

Public distribution repository for signed MOYA update manifests and release packages.

The application source code belongs in the private `machulyak/moya` repository.

## Security rules

- Never upload databases, customer data, credentials, access tokens, or private signing keys.
- The updater must verify the Ed25519 signature of the manifest before trusting release metadata.
- It must then verify the downloaded artifact's SHA-256 and size before installation.
- A missing or invalid signature must fail closed: do not install.
- The private signing key must remain offline and must never be embedded in the application.

## Update channels

- Stable: [channels/stable/manifest.json](channels/stable/manifest.json)
- Beta: [channels/beta/manifest.json](channels/beta/manifest.json)

Both channels are initialized as empty. No installable release is published yet.

## Release process

1. Build and test a Windows release from the private source repository.
2. Create the release manifest using [the schema](schemas/release-manifest.schema.json).
3. Sign the canonical UTF-8 JSON bytes of the `payload` object with the offline Ed25519 private key.
4. Publish the package and signed manifest.
5. Verify signature, SHA-256, package size, and upgrade/rollback behavior from a clean Windows test installation.
6. Promote to Stable only after Beta validation.
