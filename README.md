<div align="center">
    <img src="https://upload.wikimedia.org/wikipedia/commons/e/ea/Conda_logo.svg" alt="Conda Logo">
    <p>This repository serves as a comprehensive Conda Cheat Sheet, designed to be a handy reference for Conda commands and best practices.<br>
    Beginners are encouraged to start with <a href="https://conda.io/projects/conda/en/latest/user-guide/getting-started.html">Getting Started with Conda</a>.</p>
</div>

---

## **Table of Contents**
- [What is Conda?](#what-is-conda)
- [Prerequisites](#prerequisites)
- [Download Miniconda](#download-miniconda)
- [Installation Instructions](#installation-instructions)
  - [Windows](#windows)
  - [macOS](#macos)
  - [Linux](#linux)
- [Verifying Installation](#verifying-installation)
- [Creating a New Environment](#creating-a-new-environment)
  - [Basic Creation](#basic-creation)
  - [Listing Environments](#listing-environments)
  - [Cloning Environments](#cloning-environments)
  - [Importing from a YAML File](#importing-from-a-yaml-file)
- [Exporting an Environment](#exporting-an-environment)
- [Activating and Deactivating Environments](#activating-and-deactivating-environments)
- [Updating Environments](#updating-environments)
- [Managing Packages](#managing-packages)
  - [Installation and Uninstallation](#installation-and-uninstallation)
  - [Using Pip in Conda](#using-pip-in-conda)
  - [Listing Packages](#listing-packages)
  - [Searching for Packages](#searching-for-packages)
  - [Identifying Outdated Packages](#identifying-outdated-packages)
- [Removing an Environment](#removing-an-environment)
- [Renaming an Environment](#renaming-an-environment)
- [Managing Conda Channels](#managing-conda-channels)
- [Setting Environment Variables](#setting-environment-variables)
- [Using Docker with Conda](#using-docker-with-conda)
- [Best Practices](#best-practices)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)
- [Uninstalling Anaconda or Miniconda](#uninstalling-anaconda-or-miniconda)
- [Additional Resources](#additional-resources)

## What is Conda?
Conda is an open-source, cross-platform, language-agnostic package manager and environment management system. It facilitates the installation, upgrade, and management of software packages and their dependencies. Conda helps avoid conflicts between different package versions and environments, making it easier to resolve "dependency hell". This guide focuses on the Anaconda Distribution of Conda, which includes a large collection of scientific packages and is widely used in the data science and machine learning communities.

## Prerequisites
- Ensure you have administrative access on your computer.
- Internet connection to download Miniconda installer.

## Download Miniconda
1. Visit the [official Miniconda download page](https://docs.conda.io/en/latest/miniconda.html).
2. Select the installer appropriate for your operating system (Windows, macOS, Linux).
3. Choose between Python 3.x and 2.x versions (recommend using the latest Python 3 version).

## Installation Instructions

### Windows
1. Run the `.exe` installer and follow the on-screen instructions.
   - Choose the install location.
   - Opt to add Miniconda to your PATH (optional but recommended).
   - Choose whether to register Miniconda as your default Python.
2. Open the Command Prompt and verify the installation.

### macOS
1. Open a terminal window.
2. Navigate to the directory containing the downloaded `.sh` file.
3. Run the installer by typing `bash Miniconda3-latest-MacOSX-x86_64.sh` and follow the on-screen instructions.
4. Verify the installation and initialize Miniconda.

### Linux
1. Open a terminal window.
2. Navigate to the directory containing the downloaded `.sh` file.
3. Make the installer script executable with `chmod +x Miniconda3-latest-Linux-x86_64.sh`.
4. Run the installer by typing `./Miniconda3-latest-Linux-x86_64.sh` and follow the on-screen instructions.
5. Optionally, run `source ~/.bashrc` to refresh your shell and apply the changes.

## Verifying Installation
- Verify your installation by typing `conda list` in your terminal or Command Prompt. This command should list the installed packages.

## Creating a New Environment
### Basic Creation
To create a new environment without any default packages:
```bash
conda create --name myenv --no-default-packages
```
To specify a Python version and initial packages at creation:
```bash
conda create --name myenv python=3.8 numpy pandas
```

### Listing Environments
View all Conda environments, along with their paths:
```bash
conda env list
```

### Cloning Environments
Duplicate an existing environment, useful for creating a similar setup:
```bash
conda create --name newenv --clone baseenv
```

### Importing from a YAML File
Recreate an environment from a YAML configuration file:
```bash
conda env create -f environment.yml
```
## Exporting an Environment
Share or replicate your setup by exporting the environment to a YAML file:
```bash
conda env export > environment.yml
```

## Activating and Deactivating Environments
Switch to a specific environment or revert to the base environment:
```bash
conda activate myenv
conda deactivate
```

## Updating Environments
Keep the environment and its packages up to date:
```bash
conda update --all
```

## Managing Packages
### Installation and Uninstallation
Add or remove packages within the active environment:
```bash
conda install package_name
conda remove package_name
```

### Using `Pip` in Conda
For packages not available through Conda, or to leverage pip-specific features:
```bash
pip install package_name
pip uninstall package_name
```

### Listing Packages
Show all packages installed in the active environment:
```bash
conda list
```

### Searching for Packages
Find packages by name or pattern:
```bash
conda search 'regex_pattern'
```

### Identifying Outdated Packages
Check for available package updates:
```bash
conda search --outdated
```

For `pip`-installed packages:
```bash
pip list --outdated
```

## Removing an Environment
Delete an entire environment and its contents:
```bash
conda remove --name myenv --all
```

## Renaming an Environment
Conda does not directly support renaming, but you can clone and delete:
```bash
conda create --name newname --clone oldname
conda remove --name oldname --all
```

## Managing Conda Channels
Conda packages are downloaded from repositories called "channels". The default channel can be overridden or supplemented with additional channels to find packages not available in the default:
```bash
conda install package_name -c conda-forge
```

Add a channel to the search list permanently:
```bash
conda config --add channels channel_name
```

List all channels:
```bash
conda config --get channels
```
## Setting Environment Variables
Conda environments can have their own set of environment variables. To manage environment variables specific to an environment, you can use the `conda env config vars` command:

Set an environment variable:
```bash
conda env config vars set MY_VARIABLE=value
```

Unset an environment variable:
```bash
conda env config vars unset MY_VARIABLE
```

After setting or unsetting environment variables, you need to reactivate your environment for the changes to take effect.

## Using Conda with Docker

Conda can be used within Docker containers to manage dependencies more effectively. Here’s a basic example of a Dockerfile using Miniconda:

```bash
# Use an official Miniconda runtime as a parent image
FROM continuumio/miniconda3

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the current directory contents into the container at /usr/src/app
COPY . .

# Install any needed packages specified in environment.yml
COPY environment.yml .
RUN conda env create -f environment.yml

# Make RUN commands use the new environment:
SHELL ["conda", "run", "-n", "myenv", "/bin/bash", "-c"]

# Run app.py when the container launches
CMD ["conda", "run", "-n", "myenv", "python", "app.py"]

```

## Best Practices for Managing Environments and Packages

* **Keep environments lean:** Only install the packages you need to reduce complexity and improve environment reproducibility.
* **Use environment.yml:** Define your environment and dependencies in an `environment.yml` file for easier creation and sharing.
* **Regularly update your environments:** Keep your packages up to date with the latest versions to benefit from security patches and performance improvements.
* **Use channels cautiously:** Adding too many channels can slow down Conda operations and introduce package conflicts. Prefer well-known channels like `conda-forge`.
* **Clean up unused packages and environments:** Regularly use `conda clean --all` to remove unused packages and environments to free up space.
* **Version control for environments:** Keep `environment.yml` files under version control to track changes in your dependencies.


## Troubleshooting Common Issues

* Solving environment takes too long: Try minimizing the number of packages or using the `--no-update-deps` flag to avoid updating dependencies.
* Package conflict: Identify conflicting packages and consider manually installing specific versions or using a different channel.
* Conda command not found: Ensure Conda is properly installed and the path to Conda is added to your system's PATH environment variable.

## Uninstalling Anaconda or Miniconda
Clean uninstallation of the distribution, including all user settings and environments:
```bash
conda install anaconda-clean
anaconda-clean --yes
```

Manually remove the Anaconda or Miniconda directory afterwards.

## Uninstalling Anaconda or Miniconda

* <a href="https://docs.conda.io/en/latest/">Official Conda Documentation</a>.</p>
* <a href="https://github.com/conda/conda">Conda GitHub Repository</a>.</p>
* <a href="https://community.anaconda.cloud/">Anaconda Community Support</a>.</p>



