# E2B Computer Vision Template

**Author:** Bhagya Amarasinghe

A containerized environment for computer use capabilities with Claude, packaged as an E2B template. This is derived from Anthropic's computer use demo but optimized for E2B deployment with automated CI/CD pipelines.

## Overview

This repository provides:
- Docker container with Ubuntu 22.04, desktop environment, and Python 3.11
- noVNC web interface for remote desktop access
- Computer use tools and capabilities for Claude AI
- Automated versioning and release pipeline

## Development

### Building Locally

```bash
# Build the Docker image
docker build -f e2b.Dockerfile -t e2b-computer-vision .

# Run with development setup (requires demo files)
docker run -p 5900:5900 -p 8501:8501 -p 6080:6080 -it e2b-computer-vision
```

### Required Files for Full Build

The Dockerfile expects these directories (not included in this template repo):
- `computer_use_demo/` - Python application and requirements
- `image/` - Desktop environment configuration
- `entrypoint.sh` - Container startup script

## CI/CD Pipeline

### Automated Versioning and Releases

This repository uses semantic versioning with automated releases:

#### Conventional Commits
Use these commit message formats:
- `feat: add new feature` - Minor version bump (1.0.0 to 1.1.0)
- `fix: resolve bug` - Patch version bump (1.0.0 to 1.0.1)  
- `feat!: breaking change` - Major version bump (1.0.0 to 2.0.0)

#### PR Workflow
1. **Create PR** - Triggers validation (commit linting, Dockerfile linting)
2. **Merge to main** - Automatically creates release with version tag and changelog
3. **Docker Build** - Pushes to Google Container Registry with version tags

### Pipeline Components

**PR Validation** (`.github/workflows/pr-validation.yml`)
- Validates conventional commits
- Lints Dockerfile with Hadolint
- Automatic validation on every PR

**Release Pipeline** (`.github/workflows/release.yml`)  
- Auto-generates semantic version tags
- Creates CHANGELOG.md automatically
- Builds and pushes Docker images
- Creates GitHub releases

**Docker Registry**: `us-east4-docker.pkg.dev/jovial-branch-470306-n9/e2b-templates/computer-use-template`

### Manual Release

Trigger a manual release:
```bash
# Go to Actions → Release → Run workflow
# Or push a commit with conventional format to main
```

## Container Specifications

**Base Image**: Ubuntu 22.04  
**Python**: 3.11.6 via pyenv  
**Desktop**: Mutter window manager with UI tools  
**VNC**: noVNC web interface on port 6080  

**Included Software**:
- Firefox ESR, LibreOffice, PDF viewer
- Development tools (curl, git, build-essential)
- Image tools (imagemagick, scrot)
- Network utilities (netcat, net-tools)

## Requirements

- Docker
- GitHub repository with appropriate permissions
- Google Cloud Registry access (for automated builds)

