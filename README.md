# Environments
A repo for .yml or spec-file.txt files for python/conda environments for tasks we all have to do.

# How to you manage (conda) environments
Sources:

[Managing packages - conda 4.12.0.post35+0034690c documentation](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html)

- Managing environments
    - channels
    - python
    - virtual packages
    - etc...

[The Definitive Guide to Conda Environments](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533)

NOTE:

`pip` is a package manager *for Python.* `venv` is an environment manager *for Python*. `conda` is **both a package and environment manager** and is **language agnostic**.

## Best Practices
### Create

At the start of a project make a environment for that project

```python
# Can run just,
conda create -n myenv                                         # Just make the environment (no packages installed)
conda create -n myenv python scipy                            # Create environment with scipy installed from your primary channel
conda create -n myenv python=3.6 scipy=0.15.0 astroid babel   # Creat environment with this specific verison and packages

# also with no default packages and only installs python,
conda create --no-default-packages -n myenv python
```

- myenv is the name of the environment you’re creating for the project
- This is a best practice to avoid things overlapping/not communicating well

### Destroy

How to nuke an environment (If things go bad)

```python
conda env remove -n ENV_NAME

# If env is placed else where use:
-p **/path/to/env**
```

---

Install everything/build your environment

---

### Reproduce

To increase the reproducibility of your work create an *Environment file* which lists all the channels, packages within the environment, and where it was saved

```python
conda activate env
conda env export --file environment.yml       # Or -f
or
conda env export > environment.yml
---------------------------
# Across platforms env files:
conda env export --from-history > environment.yml
```

Environment.yml:

```python
name: null                          # Our env was made with --prefix
channels:
  - conda-forge                     # We added a third party channel
  - defaults
dependencies:
  - numpy=1.16.3=py37h926163e_0
  - opencv=3.4.2=py37h6fd60c2_1
  - pandas=0.24.2=py37h0a44026_0
  - pip=19.1.1=py37_0
  - pip:                            # Packages installed from PyPI
    - requests==2.21.0
prefix: /Users/user-name/data-science/project-name/conda-env
```

Create an env from a .yml file:

```python
conda env create -n conda-env -f /path/to/environment.yml
```

OR add all the packages from a .yml file to an existing environment:

```python
conda env update -n conda-env -f /path/to/environment.yml
```

You can also:

- Clone:
    
    ```python
    conda create --name myclone --clone myenv
    
    # Where
    # myclone = the new environment
    # myenv = the environment you want to clone
    ```
    
- Similar to the .yml version you can make a spec.txt file that has all the package into as well
    
    ```python
    conda list --explicit > spec-file.txt
    ```
    
    - An explicit spec file is not usually cross platform, and
    therefore has a comment at the top such as `# platform: osx-64`
    showing the platform where it was created.
    
    To make the environ from the spec.txt file
    
    ```python
    # Make a new env from the file:
    conda create --name myenv --file spec-file.txt
    
    # Download all the packages into an existing env:
    conda install --name myenv --file spec-file.txt
    ```
    

NOTE: **Conda environments work EXACTLY the same as python virtual env**

## How to get an Environment > Kernel and use it in jupyter notebook or lab
Sources:

[Installing the IPython kernel - IPython 8.2.0 documentation](https://ipython.readthedocs.io/en/stable/install/kernel_install.html#kernels-for-different-environments)

[GitHub - Anaconda-Platform/nb_conda_kernels: Package for managing conda environment-based kernels inside of Jupyter](https://github.com/Anaconda-Platform/nb_conda_kernels#installation)

```python
conda activate myenv                                          # source activate for older versions
conda install pip
conda install ipykernel nb_conda_kernels                      # or pip install
# add the env as a kernel to jupyter
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

- The `--name`value is used by Jupyter internally. These commands will overwrite any existing kernel with the same name. `--display-name`is what you see in the notebook menus.

**NOTE:**

What’s the difference between conda/pip install?

- Conda, astroconda, conda-forge, pip etc are different managers and allow different paths, “channels”, to install packages
- Conda manages package installation and virtual environments
- Use conda first then pip because once you use pip conda won’t be aware of the packages you pip installed

Using virtualenv or conda envs, you can make your IPython kernel in one env available to Jupyter in a different env. To do so, run ipykernel install from the kernel’s env, with –prefix pointing to the Jupyter env:

```python
/path/to/kernel/env/bin/python -m ipykernel install --prefix=/path/to/jupyter/env --name 'python-my-env'
```

NOTE: that this command will create a new configuration for the kernel in one of the preferred location (see `jupyter --paths`command for more details)

### Dealing with Kernels

List the path to all kernels

```python
jupyter kernelspec list
```

Remove a kernel with the path

```python
jupyter kernelspec uninstall unwanted_kernel
```

Edit the name:

- cd into the file listed at the end of the path and In that folder, open up file `kernel.json`
 and edit option `"display_name"`

---

---
