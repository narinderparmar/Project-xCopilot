# Release Artifacts

Project xCopilot is the canonical distribution repository. Supported binaries are published as assets on versioned GitHub Releases rather than committed directly to Git.

Every supported release must include:

- platform-specific signed installer/package and, where appropriate, portable archive;
- signed SHA-256 checksum manifest;
- release notes with supported platforms, upgrade/rollback instructions, and known issues;
- CycloneDX SBOM and third-party/model notices;
- build provenance/attestation and source commit identifier from the private build;
- verification keys/certificate identity and tested installation instructions;
- malware-scan and release-gate evidence required by the release plan.

No artifact may be labeled supported until license, distribution-rights, signing-identity, security, clean-machine install, upgrade, rollback, and provenance gates pass. Draft or test artifacts must be clearly marked and must not be presented in the getting-started guide.
