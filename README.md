# DevSecOps Portfolio & Infrastructure Hub

## Overview
This repository contains the source code, Continuous Integration (CI) pipelines, and documentation for my professional cybersecurity portfolio. It serves as the central hub for my infrastructure and security research.

## Architecture & Tech Stack
* **Frontend Framework:** Hugo (Static Site Generator)
* **CI/CD Pipeline:** GitHub Actions
* **Hosting & Delivery:** GitHub Pages
* **Version Control:** Git

## Pipeline Automation
This repository utilizes a custom GitHub Actions workflow (`hugo.yaml`) to automate deployments. 
* **Continuous Integration:** Automatically provisions a Node.js and Go environment, installs dependencies, and compiles the static HTML via Hugo upon every push to the `main` branch.
* **Continuous Deployment:** Securely deploys the build artifact to GitHub Pages utilizing temporary `GITHUB_TOKEN` permissions, eliminating the need for hardcoded deployment secrets.

## Local Development
To run this project locally for testing:
1. Clone the repository.
2. Run `hugo server` in the root directory.
3. View the live-reloading site at `localhost:1313`.

---
*Maintained by danack10*
