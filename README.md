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
cd azure-artifacts-python-helloworld

# Run the hello world script
python app/hello.py
```

### 2. Run as a Module

```bash
# Run as Python module
python -m app.hello
```

### 3. Import and Use

```python
from app.hello import say_hello

# Use the function
print(say_hello("Your Name"))
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

## Azure DevOps Integration

This project includes `azure-pipelines.yml` for CI/CD integration with Azure Artifacts.

### Setup Steps:
1. Create an Azure DevOps project
2. Create an Azure Artifacts feed
3. Update the feed name in `azure-pipelines.yml`
4. Run the pipeline to build and publish your package

### Installing from Azure Artifacts:

```bash
pip install azurehello --extra-index-url https://pkgs.dev.azure.com/<ORG>/<PROJECT>/_packaging/<FEED>/pypi/simple/
```

## What This Project Demonstrates

- ✅ Basic Python package structure
- ✅ Setup.py configuration for packaging
- ✅ Azure DevOps pipeline integration
- ✅ Publishing to Azure Artifacts
- ✅ Package installation and usage

