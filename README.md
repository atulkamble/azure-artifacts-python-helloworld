# Azure Artifacts Python Hello World

A simple Python Hello World project that demonstrates how to build and publish packages to Azure Artifacts.

## Project Structure

```
azure-artifacts-python-helloworld/
├── app/
│   ├── __init__.py
│   └── hello.py
├── setup.py
├── requirements.txt
├── azure-pipelines.yml
└── README.md
```

## Quick Start

### 1. Run the Code Directly

```bash
# Navigate to project directory
git clone https://github.com/atulkamble/azure-artifacts-python-helloworld.git
cd azure-artifacts-python-helloworld

# Run the hello world script
python app/hello.py
```

### 2. Run as a Module 

```bash
# Run as Python module
python -m app.hello
```

### 3. One Line Import and Execute

```python
python -c "from app.hello import say_hello; print(say_hello('Your Name'))"
```

## Local Development

### Create Virtual Environment (Recommended)

```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # On macOS/Linux

# venv\Scripts\activate    # On Windows
```

### Build and Install Package Locally

```bash
# Install build tools
pip install setuptools wheel

# Build the package
python setup.py sdist bdist_wheel

# Install the package locally
pip install dist/azurehello-1.0.0-py3-none-any.whl

# Test the installed package
python -c "from app.hello import say_hello; print(say_hello('Local Test'))"
```

## Azure DevOps Pipeline Setup & Configuration

### Step 1: Prerequisites Setup

#### 1.1 Create Azure DevOps Organization & Project
1. Go to [Azure DevOps](https://dev.azure.com)
2. Create a new organization or use existing one
3. Create a new project (e.g., "python-artifacts-demo")

#### 1.2 Create Azure Artifacts Feed
1. In your Azure DevOps project, go to **Artifacts**
2. Click **"Create Feed"**
3. Feed name: `python-packages` (or your preferred name)
4. Visibility: **Project** (recommended for demo)
5. Click **"Create"**

### Step 2: Pipeline Configuration

#### 2.1 Upload Code to Azure Repos
```bash
# Clone your repo and push to Azure DevOps
git remote add azure https://dev.azure.com/{YOUR_ORG}/{YOUR_PROJECT}/_git/{YOUR_REPO}
git push azure main
```

#### 2.2 Update azure-pipelines.yml Configuration
Replace `your-feed-name` with your actual feed name:

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:

# Step 1: Install Python
- task: UsePythonVersion@0
  inputs:
    versionSpec: '3.x'
  displayName: "Use Python 3.x"

# Step 2: Install setuptools + wheel + twine
- script: |
    pip install setuptools wheel twine
  displayName: "Install packaging tools"

# Step 3: Build Python package
- script: |
    python setup.py sdist bdist_wheel
  displayName: "Build Python package"

# Step 4: Publish Python package to Azure Artifacts
- task: TwineAuthenticate@1
  inputs:
    artifactFeed: "python-packages"  # Replace with your feed name
  displayName: "Authenticate with Azure Artifacts"

- script: |
    python -m twine upload -r "python-packages" --config-file $(PYPIRC_PATH) dist/*
  displayName: "Upload package to Azure Artifacts"

# Step 5: Install package from Azure Artifacts (test)
- script: |
    pip install azurehello --extra-index-url https://pkgs.dev.azure.com/{YOUR_ORG}/{YOUR_PROJECT}/_packaging/python-packages/pypi/simple/
    python -c "import app.hello as h; print(h.say_hello('Pipeline Test'))"
  displayName: "Install & test package"
```

### Step 3: Create and Run Pipeline

#### 3.1 Create Pipeline
1. In Azure DevOps, go to **Pipelines**
2. Click **"Create Pipeline"**
3. Select **"Azure Repos Git"**
4. Choose your repository
5. Select **"Existing Azure Pipelines YAML file"**
6. Choose `/azure-pipelines.yml`
7. Click **"Continue"**

#### 3.2 Configure Pipeline Variables
1. Before running, click **"Variables"**
2. Add these variables if needed:
   - `YOUR_ORG`: Your Azure DevOps organization name
   - `YOUR_PROJECT`: Your project name
   - `FEED_NAME`: Your artifacts feed name

#### 3.3 Run Pipeline
1. Click **"Run"** to execute the pipeline
2. Monitor each step:
   - ✅ **Step 1**: Python installation
   - ✅ **Step 2**: Install packaging tools
   - ✅ **Step 3**: Build Python package
   - ✅ **Step 4**: Authenticate & upload to Azure Artifacts
   - ✅ **Step 5**: Test installation from feed

### Step 4: Verify Package in Azure Artifacts

1. Go to **Artifacts** in Azure DevOps
2. Click on your feed (`python-packages`)
3. You should see `azurehello` package version 1.0.0
4. Click on the package to see details and download options

### Step 5: Install Package from Azure Artifacts

#### 5.1 Get Feed URL
1. In Artifacts, click **"Connect to Feed"**
2. Select **"pip"**
3. Copy the index URL

#### 5.2 Install Package
```bash
# Install from your Azure Artifacts feed
pip install azurehello --extra-index-url https://pkgs.dev.azure.com/{YOUR_ORG}/{YOUR_PROJECT}/_packaging/python-packages/pypi/simple/

# Test the installed package
python -c "from app.hello import say_hello; print(say_hello('Azure Artifacts'))"
```

### Step 6: Pipeline Monitoring & Troubleshooting

#### 6.1 Monitor Pipeline Runs
- Go to **Pipelines** → **Recent runs**
- Check logs for each step if issues occur

#### 6.2 Common Issues & Solutions
- **Authentication failed**: Ensure feed permissions are set correctly
- **Package already exists**: Update version in `setup.py`
- **Build failed**: Check Python syntax and dependencies

### Pipeline Workflow Visualization

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Code Commit   │───▶│  Trigger Build  │───▶│  Install Python │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                        │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Upload Package  │◀───│  Build Package  │◀───│ Install Tools   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
        │
        ▼
┌─────────────────┐    ┌─────────────────┐
│ Test & Verify   │───▶│   Deployment    │
└─────────────────┘    └─────────────────┘
```

## What This Project Demonstrates

- ✅ Basic Python package structure
- ✅ Setup.py configuration for packaging
- ✅ Azure DevOps pipeline integration
- ✅ Publishing to Azure Artifacts
- ✅ Package installation and usage

