---
layout: post
title: "anakonda-jupyter"
date: 2024-09-28
categories: [development]
tags: [anaconda, jupyter]
---

### **Anaconda Environment & Jupyter Cheat Sheet**

#### **1. Create a new environment:**
```
conda create --name <env_name> <package_name>
```
- Example: `conda create --name myenv python=3.9 numpy`

#### **2. Activate an environment:**
```
conda activate <env_name>
```
- Example: `conda activate myenv`

#### **3. Deactivate the current environment:**
```
conda deactivate
```

#### **4. List all environments:**
```
conda env list
```
or
```
conda info --envs
```

#### **5. Clone an existing environment:**
```
conda create --name <new_env_name> --clone <existing_env_name>
```
- Example: `conda create --name cloned_env --clone base`

#### **6. Remove an environment:**
```
conda remove --name <env_name> --all
```
- Example: `conda remove --name myenv --all`

#### **7. Install packages into an environment:**
```
conda install --name <env_name> <package_name>
```
- Example: `conda install --name myenv pandas`

#### **8. Remove a package from an environment:**
```
conda remove --name <env_name> <package_name>
```
- Example: `conda remove --name myenv numpy`

#### **9. Update all packages in an environment:**
```
conda update --name <env_name> --all
```

#### **10. Export an environment to a `.yml` file (for sharing):**
```
conda env export --name <env_name> > environment.yml
```

#### **11. Create an environment from an `.yml` file:**
```
conda env create -f environment.yml
```

#### **12. Check details about a specific environment:**
```
conda list --name <env_name>
```
- Example: `conda list --name myenv`

#### **13. Update Conda itself:**
```
conda update conda
```

#### **14. Search for available packages:**
```
conda search <package_name>
```

---

### **Jupyter Notebook Commands:**

#### **1. Install Jupyter Notebook and Jupyter lab in an environment:**
```
conda install ipykernel notebook
```

#### **2. Launch Jupyter Notebook:**
```
jupyter notebook
```

#### **3. Launch JupyterLab:**
```
jupyter lab
```

#### **4. Create a Jupyter kernel for a Conda environment:**
This allows you to use the environment in Jupyter:
```
python -m ipykernel install --user --name=<env_name>
```
- Example: `python -m ipykernel install --user --name=myenv`

Than you can select the kernel in Jupyter Notebook or Jupyter Lab.

#### **5. List available Jupyter kernels:**
```
jupyter kernelspec list
```

#### **6. Remove a Jupyter kernel:**
```
jupyter kernelspec uninstall <kernel_name>
```

#### **7. Stop the Jupyter Notebook/Lab server:**
To stop a running instance of Jupyter, press `CTRL + C` in the terminal where it’s running.

#### **8. Convert a Jupyter notebook to a different format (e.g., HTML, PDF):**
```
jupyter nbconvert --to <format> <notebook.ipynb>
```
- Example: `jupyter nbconvert --to html my_notebook.ipynb`

#### **9. Run a Jupyter notebook from the command line:**
```
jupyter nbconvert --execute <notebook.ipynb>
```

#### **10. List running Jupyter notebook servers:**
```
jupyter notebook list
```

#### **11. Shut down a running Jupyter notebook server:**
```
jupyter notebook stop <port_number>
```
- Example: `jupyter notebook stop 8888`

---

### **Shortcuts:**
- **List packages in the current environment:**
  ```
  conda list
  ```

- **Install a package in the active environment:**
  ```
  conda install <package_name>
  ```