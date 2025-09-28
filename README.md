
# **Advanced GitHub Actions and CI/CD Best Practices**

## Project Overview

This project focuses on mastering advanced GitHub Actions and CI/CD workflows. It emphasizes writing maintainable, efficient, and secure automation pipelines while optimizing performance and modularity for complex software projects.

## Why is this Project Relevant

As software projects grow in complexity, effective CI/CD pipelines become critical to ensure rapid, reliable, and secure deployments. This project equips developers with advanced skills to build robust and scalable workflows, improving productivity and software quality.

## Project Goals and Objectives

* Build maintainable and modular GitHub Actions workflows.
* Optimize workflow performance through caching and parallelization.
* Implement security best practices, including secrets management and least privilege principles.
* Gain hands-on experience in designing professional CI/CD pipelines for real-world projects.

## Prerequisites

* Basic knowledge of GitHub Actions and CI/CD concepts.
* Familiarity with Git, YAML, and command-line interface.
* Node.js or similar development environment (for demonstration workflows).

## Project Deliverables

* Fully structured project directory with necessary files.
* Reusable and maintainable workflow examples.
* Optimized and secure GitHub Actions pipelines.
* Documentation (README.md) with detailed instructions and best practices.

## Tools & Technologies Used

* **GitHub Actions** for CI/CD automation.
* **Git** for version control.
* **Node.js** or any target application environment for workflow testing.
* **YAML** for workflow configuration.
* **Text Editor/IDE** (VSCode recommended).

## Project Components

1. Project Directory and Sub-Directories
2. GitHub Actions Workflows (`.github/workflows`)
3. README.md Documentation
4. Images Folder (`images/`) for screenshots and diagrams
5. Modular and reusable workflow scripts

## Task 1: Project Setup and Directory Structure

### Objective

Create the root project directory, essential sub-directories, and initialize required files for workflow development and documentation.

### Steps

1. **Create Project Root Directory**

```bash
mkdir advanced-github-actions
cd advanced-github-actions
```

2. **Create Sub-Directories**

```bash
mkdir .github
mkdir .github/workflows
mkdir images
```

3. **Initialize Essential Files**

```bash
touch README.md
touch .gitignore
```

4. **Example .gitignore Entries**

```
node_modules/
*.log
.env
```

5. **Verify Directory Structure**

```
advanced-github-actions/
├── .github/
│   └── workflows/
├── images/
├── README.md
└── .gitignore
```

✅ **Task 1 Complete** – Project structure ready for workflow development.

## Task 2: Create Initial GitHub Actions Workflow

### Objective

Set up the first GitHub Actions workflow to demonstrate a **basic CI process**. This will serve as the foundation for building modular, maintainable, and secure pipelines throughout the project.

### Steps

1. **Create Workflow File**
   Inside the `.github/workflows` directory, create a file named `build.yml`:

```bash
cd .github/workflows
touch build.yml
```

2. **Define a Basic Workflow**
   Add the following content to `build.yml`:

```yaml
name: Basic CI Workflow

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Run setup check
      run: echo "✅ Repository checked out successfully!"

    - name: Run sample validation
      run: echo "🚀 Basic CI workflow is running without Node.js dependencies."
```

3. **Workflow Explanation**

* **Triggers:** Runs automatically on `push` and `pull_request` to the `main` branch.
* **Jobs:**

  * `build` runs on the latest Ubuntu runner.
* **Steps:**

  1. **Checkout** – Retrieves the repository code.
  2. **Run setup check** – Verifies repository setup.
  3. **Run sample validation** – Executes a simple validation command (simulated test).

4. **git init, add, Commit and Push Workflow**

```bash
git init
git add .github/workflows/build.yml
git commit -m "Add initial basic CI workflow"
git branch -M main
git remote add origin https://github.com/Holuphilix/advanced-github-actions.git
git push -u origin main
```

5. **Verify Workflow**

* Go to your repository on GitHub.
* Navigate to the **Actions** tab.
* Confirm the workflow runs successfully when code is pushed or a pull request is created.

✅ **Task 2 Complete** – A working CI pipeline is now active and independent of Node.js.

## Task 3: Modular Workflows and Performance Optimization

### Objective

Enhance your GitHub Actions workflows by creating **reusable modules** and implementing **caching** to reduce build times and improve efficiency.

### Steps

### 1. **Create a Reusable Workflow for Node.js Testing**

1. Inside `.github/workflows/`, create a file named `nodejs-test.yml`:

```bash
cd .github/workflows
touch nodejs-test.yml
```

2. Add the following content to `nodejs-test.yml` for a modular testing workflow:

```yaml
name: Node.js Test Workflow

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: Cache npm dependencies
      uses: actions/cache@v3
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-node-

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```

**Explanation:**

* `workflow_call` allows this workflow to be reused by other workflows.
* `actions/cache` caches Node.js dependencies to speed up subsequent runs.

### 2. **Update Main Workflow to Use Reusable Workflow**

1. Open your `build.yml` workflow from Task 2.
2. Modify it to call the reusable workflow:

```yaml
name: Build and Test Node.js Application

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  call-nodejs-test:
    uses: ./.github/workflows/nodejs-test.yml
```

**Explanation:**

* Instead of repeating steps, the main workflow now **calls the modular workflow**.
* This improves maintainability and reduces duplication.

### 3. **Commit and Push Changes**

```bash
git add .github/workflows/nodejs-test.yml .github/workflows/build.yml
git commit -m "Add reusable Node.js test workflow and implement caching"
git push origin main
```

### 4. **Verify Workflow**

* Go to the **Actions** tab in your repository.
* Trigger a push or pull request.
* Ensure the `build.yml` workflow runs successfully by **calling the reusable `nodejs-test.yml` workflow**.
* Confirm that caching reduces install times for dependencies on subsequent runs.

✅ **Task 3 Complete** – Workflows are now **modular** and **performance-optimized**.

