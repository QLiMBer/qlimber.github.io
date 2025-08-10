---
layout: post
title: "Git submodules 2"
date: 2024-09-26
categories: [development]
tags: [git]
---


### Cheat Sheet: Managing Shared Files Across Branches Using Git Submodule

#### **Objective:**
You want to maintain shared files (`AI_assist/future_comments.txt`, `AI_assist/prompts.md`, `README.md`) across multiple branches (`master`, `working`, and future branches). By using **Git submodules**, changes made in the shared files branch (`shared-files`) are reflected in all other branches.

---

### **Steps to Set Up and Manage Shared Files via Submodules:**

#### 1. **Create a Separate Branch (`shared-files`) for Shared Files**:
   - Switch to the `shared-files` branch:
     ```bash
     git checkout -b shared-files
     ```
   - Remove unnecessary files from the root of this branch and ensure only the required shared files are in this branch.
     ```bash
     git rm -r <unwanted-files>
     git commit -m "Remove unnecessary files from shared-files branch"
     ```
   - Commit the desired shared files (`AI_assist/future_comments.txt`, `README.md`, etc.):
     ```bash
     git add <shared-files>
     git commit -m "Add shared files to shared-files branch"
     ```

   - Push the `shared-files` branch to the remote:
     ```bash
     git push origin shared-files
     ```

#### 2. **Add `shared-files` as a Submodule in Other Branches (e.g., `master`, `working`)**:
   - Checkout your `master` or `working` branch:
     ```bash
     git checkout master
     ```

   - Add the `shared-files` branch as a submodule:
     ```bash
     git submodule add -b shared-files https://github.com/your-username/your-repo.git shared-files
     ```
     > **Note:** Replace `your-username/your-repo.git` with your repository URL.

   - Commit the submodule:
     ```bash
     git commit -m "Add shared-files as a submodule"
     ```

   - Push the changes to the remote repository:
     ```bash
     git push origin master
     ```

#### 3. **Syncing Changes Across Branches**:
   - Whenever you make changes to the files in the `shared-files` branch, they won’t automatically be reflected in other branches. You need to **update the submodule** in each branch to pull the latest changes.

   **How to Update the Submodule**:
   - To pull the latest changes from the `shared-files` branch into other branches (e.g., `master`, `working`):
     ```bash
     git submodule update --remote
     ```

   - Commit and push the updated submodule:
     ```bash
     git commit -m "Update shared-files submodule"
     git push origin master
     ```

---

### **Key Points and Things to Avoid:**
1. **Avoid Nested or Duplicated Folders**:
   - Ensure that shared files exist **only once** in the `shared-files` folder (submodule). Avoid duplicating them in the root directory or elsewhere.

2. **Always Run `git submodule update --remote`**:
   - When switching between branches, or after making changes in the `shared-files` branch, always run `git submodule update --remote` in your target branch to ensure the submodule is updated.

3. **Commit Submodule Changes**:
   - After updating the submodule in a branch, don’t forget to commit the changes in the parent branch (`master`, `working`, etc.).

4. **Use the `--force` Option if Necessary**:
   - If you encounter an error while adding a submodule that already exists, use the `--force` flag to reuse the existing submodule:
     ```bash
     git submodule add --force -b shared-files https://github.com/your-username/your-repo.git shared-files
     ```

---

### **Commands Summary**:

- **Create shared-files branch**:  
  ```bash
  git checkout -b shared-files
  ```

- **Add submodule**:  
  ```bash
  git submodule add -b shared-files https://github.com/your-username/your-repo.git shared-files
  ```

- **Update submodule**:  
  ```bash
  git submodule update --remote
  ```

- **Commit submodule changes**:  
  ```bash
  git commit -m "Update shared-files submodule"
  ```

- **Push changes**:  
  ```bash
  git push origin master
  ```

---

### **End Result**:
- You now have a working setup where `shared-files` is correctly linked as a submodule across all branches (`master`, `working`, etc.).
- Changes made to the `shared-files` branch are easily synchronized across other branches using `git submodule update --remote`.

Let me know if you need further modifications to the cheat sheet!
