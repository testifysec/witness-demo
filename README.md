# Witness Demo - Docker Build with Provenance

This demo shows how to build a container image with Docker Buildx while capturing provenance using Witness, signing with Sigstore, and uploading attestations to Archivista.

## Overview

The project includes:
- A simple Go HTTP server application
- Multi-stage Dockerfile for efficient container builds
- GitHub Actions workflow that:
  - Builds multi-platform container images (linux/amd64, linux/arm64)
  - Pushes to GitHub Container Registry (GHCR)
  - Captures build provenance with Witness
  - Signs attestations with Sigstore (using default TSA)
  - Uploads attestations to Archivista for public verification

## Prerequisites

- GitHub repository with Actions enabled
- No additional secrets needed (uses GITHUB_TOKEN for GHCR)

## How It Works

1. **Docker Buildx**: Builds the container with metadata output and SLSA provenance
2. **Witness Run Action**: Wraps the build command to capture:
   - Command execution details
   - Environment information
   - Input/output materials
   - Build artifacts
3. **Sigstore**: Signs the attestation using GitHub OIDC identity
4. **Archivista**: Stores the attestation for later verification

## Usage

1. Push code to the `main` branch or create a PR
2. GitHub Actions will automatically:
   - Build the container image
   - Push to `ghcr.io/<your-username>/witness-demo`
   - Create and sign attestations
   - Upload to Archivista

## Verifying Attestations

After the build completes, you can verify the attestations:

```bash
# The attestation will be available at Archivista
# Check the GitHub Actions logs for the specific attestation ID
```

## Container Image

The built image will be available at:
```
ghcr.io/<your-github-username>/witness-demo:latest
ghcr.io/<your-github-username>/witness-demo:<commit-sha>
```

## Security Features

- **Sigstore Signing**: Uses ephemeral keys with GitHub OIDC identity
- **Timestamp Authority**: Uses FreeTSA for trusted timestamps
- **Public Transparency**: Attestations are publicly verifiable via Archivista
- **SLSA Provenance**: Docker Buildx generates SLSA provenance attestations
- **SBOM Generation**: Automatic software bill of materials creation