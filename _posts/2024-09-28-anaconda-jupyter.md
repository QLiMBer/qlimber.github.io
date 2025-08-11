---
layout: post
title: "anakonda-jupyter"
date: 2024-09-28
categories: [development]
tags: [anaconda, jupyter]
---

### **Anaconda Environment & Jupyter Cheat Sheet**

#### **1. Create a new environment:**

```bash
conda create --name <env_name> <package_name>
```

- Example: `conda create --name myenv python=3.9 numpy`

#### **2. Activate an environment:**

```bash
conda activate <env_name>
```

- Example: `conda activate myenv`

#### **3. Deactivate the current environment:**

```bash
conda deactivate
```

#### **4. List all environments:**

```bash
conda env list
```

or

```bash
conda info --envs
```

#### **5. Clone an existing environment:**

```bash
conda create --name <new_env_name> --clone <existing_env_name>
```

- Example: `conda create --name cloned_env --clone base`

#### **6. Remove an environment:**

```bash
conda remove --name <env_name> --all
```

- Example: `conda remove --name myenv --all`

#### **7. Install packages into an environment:**

```bash
conda install --name <env_name> <package_name>
```

- Example: `conda install --name myenv pandas`

#### **8. Remove a package from an environment:**

```bash
conda remove --name <env_name> <package_name>
```

- Example: `conda remove --name myenv numpy`

#### **9. Update all packages in an environment:**

```bash
conda update --name <env_name> --all
```

#### **10. Export an environment to a `.yml` file (for sharing):**

```bash
conda env export --name <env_name> > environment.yml
```

#### **11. Create an environment from an `.yml` file:**

```bash
conda env create -f environment.yml
```

#### **12. Check details about a specific environment:**

```bash
conda list --name <env_name>
```

- Example: `conda list --name myenv`

#### **13. Update Conda itself:**

```bash
conda update conda
```

#### **14. Search for available packages:**

```bash
conda search <package_name>
```

---

### **Jupyter Notebook Commands:**

#### **1. Install Jupyter Notebook and Jupyter lab in an environment:**

```bash
conda install ipykernel notebook
```

#### **2. Launch Jupyter Notebook:**

```bash
jupyter notebook
```

#### **3. Launch JupyterLab:**

```bash
jupyter lab
```

#### **4. Create a Jupyter kernel for a Conda environment:**

This allows you to use the environment in Jupyter:

```bash
python -m ipykernel install --user --name=<env_name>
```

- Example: `python -m ipykernel install --user --name=myenv`

Than you can select the kernel in Jupyter Notebook or Jupyter Lab.

#### **5. List available Jupyter kernels:**

```bash
jupyter kernelspec list
```

#### **6. Remove a Jupyter kernel:**

```bash
jupyter kernelspec uninstall <kernel_name>
```

#### **7. Stop the Jupyter Notebook/Lab server:**

To stop a running instance of Jupyter, press `CTRL + C` in the terminal where it’s running.

#### **8. Convert a Jupyter notebook to a different format (e.g., HTML, PDF):**

```bash
jupyter nbconvert --to <format> <notebook.ipynb>
```

- Example: `jupyter nbconvert --to html my_notebook.ipynb`

#### **9. Run a Jupyter notebook from the command line:**

```bash
jupyter nbconvert --execute <notebook.ipynb>
```

#### **10. List running Jupyter notebook servers:**

```bash
jupyter notebook list
```

#### **11. Shut down a running Jupyter notebook server:**

```bash
jupyter notebook stop <port_number>
```

- Example: `jupyter notebook stop 8888`

---

### **Shortcuts:**

- **List packages in the current environment:**

  ```bash
  conda list
  ```

- **Install a package in the active environment:**

  ```bash
  conda install <package_name>
  ```
