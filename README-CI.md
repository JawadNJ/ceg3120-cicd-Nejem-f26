
# Continuous Integration Project Overview

## Project Goal

The goal of this project is to create an automated Continuous Integration (CI) process that builds and publishes Docker container images to Docker Hub. When I make changes to my website, commit the changes, and push a new semantic version tag to GitHub, GitHub Actions automatically builds a new Docker image and publishes it to my Docker Hub repository.

This process makes it possible to maintain multiple versions of my website and use an older version if I need to roll back.

## Tools Used

- **Visual Studio Code:** Used to edit the website files, Dockerfile, GitHub Actions workflow, and project documentation.
- **Git:** Used for version control, saving changes, creating commits, and creating semantic version tags.
- **GitHub:** Stores my project repository and triggers GitHub Actions when I push a version tag.
- **GitHub Actions:** Automates the process of building and publishing Docker images.
- **Docker:** Used to build container images and run the website in containers.
- **Docker Hub:** Stores the published Docker images and their different version tags.
- **Docker Metadata Action:** Generates image tags such as `latest`, `1`, and `1.1`.
- **Docker Buildx:** Provides the build environment used by the GitHub Actions workflow.

## Continuous Integration Process Diagram

The diagram below shows how I make changes to my website, create a version tag, and use GitHub Actions to automatically build and publish a Docker image to Docker Hub.

```mermaid
flowchart LR
    subgraph DEV["1. LOCAL COMPUTER"]
        A["Edit website files<br/>HTML / CSS"]
        B["Commit and push<br/>code to GitHub"]
        C["Create and push<br/>version tag<br/>v1.1.0"]
        A --> B --> C
    end

    subgraph GH["2. GITHUB ACTIONS"]
        D["GitHub receives<br/>version tag"]
        E["Workflow starts<br/>on tag push"]
        F["Check out<br/>repository"]
        G["Log in to Docker Hub<br/>using GitHub Secrets"]
        H["Set up Docker Buildx<br/>and generate tags"]
        I["Build Docker image<br/>and push to Docker Hub"]
        D --> E --> F --> G --> H --> I
    end

    subgraph DH["3. DOCKER HUB"]
        J["tech-hub<br/>Container Image Repository"]
        K["Published image tags<br/><br/>latest<br/>1<br/>1.1"]
        L["Previous version retained<br/>1.0<br/><br/>Available for rollback"]
        J --> K
        J --> L
    end

    C --> D
    I --> J

    N["Important: A regular code push does not start this workflow.<br/>Only pushing a version tag such as v1.1.0 starts the publishing process."]
    B -.-> N

    classDef local fill:#E8F1FF,stroke:#3973C6,color:#17365D,stroke-width:1.5px
    classDef github fill:#F0EAFE,stroke:#7954B8,color:#39245F,stroke-width:1.5px
    classDef docker fill:#E5F6F3,stroke:#238777,color:#124D45,stroke-width:1.5px
    classDef note fill:#FFF5DB,stroke:#D49A27,color:#684B12,stroke-width:1.5px

    class A,B,C local
    class D,E,F,G,H,I github
    class J,K,L docker
    class N note

    style DEV fill:#F7FAFF,stroke:#3973C6,stroke-width:2px
    style GH fill:#FAF8FF,stroke:#7954B8,stroke-width:2px
    style DH fill:#F5FCFA,stroke:#238777,stroke-width:2px
```

## Project Status

The project is working as expected. I successfully published two versions of my website using Git tags:

- `v1.0.0`: Original Tech Hub website.
- `v1.1.0`: Updated Tech Hub website with a new release banner.

I verified that both versions can be downloaded from Docker Hub and run in separate containers. The older version remains available for rollback.

# Project 6 – Continuous Integration and Semantic Versioning

## 1. Generating Git Tags

Git tags identify specific commits as releases. I used semantic versioning to organize my releases.

Semantic versioning follows this format:

`MAJOR.MINOR.PATCH`

- **Major:** Used for major or breaking changes.
- **Minor:** Used when adding new features.
- **Patch:** Used for bug fixes.

### Viewing Existing Tags

To see all tags in my Git repository, I used:

```powershell
git tag
```

### Creating a Tag

To create an annotated tag for a release, I used:

```powershell
git tag -a v1.0.0 -m "Release version 1.0.0"
```

For my second release, I used:

```powershell
git tag -a v1.1.0 -m "Release version 1.1.0"
```

### Pushing Tags to GitHub

To upload a tag to GitHub and trigger the workflow, I used:

```powershell
git push origin v1.0.0
```

For the second release:

```powershell
git push origin v1.1.0
```

## 2. Semantic Versioning Container Images with GitHub Actions

I modified my `docker-publish.yml` workflow to use Git tags instead of running whenever code is pushed to the main branch.

### Workflow Trigger

The workflow uses the following trigger:

```yaml
on:
  push:
    tags:
      - 'v*.*.*'
```

This means the workflow runs whenever a version tag matching this pattern is pushed to GitHub, such as `v1.0.0` or `v1.1.0`. It does not run on ordinary code pushes.

### Workflow Steps

The workflow performs these tasks:

1. Checks out the GitHub repository using `actions/checkout`.
2. Logs in to Docker Hub using the `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` repository secrets.
3. Sets up Docker Buildx.
4. Uses `docker/metadata-action` to generate Docker image tags from the Git version.
5. Uses `docker/build-push-action` to build the Docker image and publish it to Docker Hub.

### Generated Docker Image Tags

The metadata action generates three tags:

- `latest` – Always points to the newest release.
- `1` – Represents the major version.
- `1.0` – Represents the major and minor version.

For example, version `v1.1.0` generates `latest`, `1`, and `1.1`.

### Values to Update for Another Repository

If this workflow is used in another project, these values may need to be changed:

- **Docker Hub repository:** Update the image name in the `images` setting and the Docker image repository name.
- **GitHub secrets:** Create `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` in the new GitHub repository.
- **Build context:** Update `context: .` if the Dockerfile or application files are located somewhere else.
- **Workflow trigger:** I Changed the tag pattern if a different version naming format is used.

The Docker Hub username and repository name must match the actual Docker Hub account and repository.

### Workflow File

[View my Docker Publish Workflow](https://github.com/JawadNJ/ceg3120-cicd-Nejem-f26/blob/main/.github/workflows/docker-publish.yml)

## 3. Testing and Validating

### Testing GitHub Actions

I tested the workflow by pushing version tags to GitHub. I checked the Actions tab and confirmed that both releases completed successfully with a green check mark.

### Verifying Docker Hub Tags

I checked my Docker Hub repository and confirmed that the following tags were published:

- `latest`
- `1`
- `1.0`
- `1.1`

### Testing the Docker Images

I pulled both released versions from Docker Hub:

```powershell
docker pull jojonij/tech-hub:1.0
docker pull jojonij/tech-hub:1.1
```

I then ran each version in a separate container:

```powershell
docker run -d -p 8081:80 --name tech-hub-v1 jojonij/tech-hub:1.0

docker run -d -p 8082:80 --name tech-hub-v1-1 jojonij/tech-hub:1.1
```

I opened both websites in my browser:

- Version 1.0: `http://localhost:8081`
- Version 1.1: `http://localhost:8082`

I confirmed that version 1.0 displayed the original website and version 1.1 displayed the updated website with the new release banner.

This demonstrated that older Docker image versions remain available and can be used for rollback.

### Docker Hub Repository

[View my Docker Hub Repository](https://hub.docker.com/repository/docker/jojonij/tech-hub/general)

## 4. Screenshots and Evidence

The following screenshots are saved in the `images/screenshots/` directory:

- `version-1.0.png` – Original website running with Docker image version 1.0.
- `version-1.1.png` – Updated website running with Docker image version 1.1.
- `dockerhub-version-tags.png` – Docker Hub showing the published version tags.
- `github-version-1.1.png` – Successful GitHub Actions workflow for version 1.1.0.

These screenshots provide evidence that the workflow successfully builds, publishes, and maintains multiple versions of my Docker image.

# Resources

The following resources helped me complete this project:

- GitHub Actions Documentation: Used to understand how GitHub Actions workflows are configured and executed.

- Docker Metadata Action: Used to generate semantic version tags for Docker images.

- Docker Manage Tags and Labels: Used to understand how to build and publish Docker images with multiple tags.

- Semantic Versioning: Used to understand major, minor, and patch version numbers.

## Generative AI Assistance

I used OpenAI ChatGPT to help understand the project requirements and complete the implementation.

Some of the prompts I used included:

- "Explain what each section of the GitHub Actions YAML file does."
- "Help me troubleshoot the Docker username and password required error."
- "Explain how to use Git tags for semantic versioning."

ChatGPT was used for explanations, troubleshooting, workflow guidance, and documentation assistance. I performed the project setup, ran the commands, tested the Docker containers, and verified the published images myself.