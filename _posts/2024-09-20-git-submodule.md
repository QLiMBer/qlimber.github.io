---
layout: post
title: "Git submodules"
date: 2024-09-20
categories: [development]
tags: [git]
---

### Cheat Sheet: Setting Up and Managing a Shared Submodule Across Branches

#### **Purpose:**
You need to share specific files (`AI_assist/future_comments.txt`, `AI_assist/prompts.md`, `README.md`) across multiple branches (`master`, `working`, and potentially others in the future). Using a **Git submodule**, these files are maintained in a separate branch (`shared-files`), and the submodule is added to all relevant branches to ensure synchronization. This method allows edits in one branch to automatically reflect in all other branches that use the submodule.

---

### **Steps to Achieve the Setup Using Submodules**

#### 1. **Prepare the `shared-files` Branch**:
   - This branch will contain only the specific files that need to be shared across other branches.
   
   Steps:
   - Switch to the branch (e.g., `master`):
     ```bash
     git checkout master
     ```
   - Create the `shared-files` branch:
     ```bash
     git checkout -b shared-files
     ```
   - Remove all unnecessary files from tracking:
     ```bash
     git rm -r --cached .
     ```
   - Add back only the files you need:
     ```bash
     git add AI_assist/future_comments.txt AI_assist/prompts.md README.md
     ```
   - Commit the changes:
     ```bash
     git commit -m "Initial commit of shared files"
     ```
   - Push the `shared-files` branch to the remote:
     ```bash
     git push -u origin shared-files
     ```

#### 2. **Add the `shared-files` Submodule to the `master` Branch**:

   - Switch to the `master` branch:
     ```bash
     git checkout master
     ```
   - Remove the duplicate files from the `master` branch (if present):
     ```bash
     git rm AI_assist/future_comments.txt AI_assist/prompts.md README.md
     git commit -m "Remove duplicate files before adding submodule"
     ```
   - Add the `shared-files` branch as a submodule in the `master` branch:
     ```bash
     git submodule add -b shared-files https://github.com/your-username/your-repo.git shared-files
     ```
     > **Note:** Replace `https://github.com/your-username/your-repo.git` with your repository URL.

   - Commit the submodule addition:
     ```bash
     git commit -m "Add shared-files as submodule in master"
     ```

#### 3. **Repeat the Same for the `working` Branch**:
   - Switch to the `working` branch:
     ```bash
     git checkout working
     ```
   - Remove the duplicate files from `working`:
     ```bash
     git rm AI_assist/future_comments.txt AI_assist/prompts.md README.md
     git commit -m "Remove duplicate files before adding submodule"
     ```
   - Add the submodule to the `working` branch:
     ```bash
     git submodule add -b shared-files https://github.com/your-username/your-repo.git shared-files
     ```

   - **If you encounter a "submodule already exists" error**, use the `--force` option to reuse the existing submodule:
     ```bash
     git submodule add -b shared-files --force https://github.com/your-username/your-repo.git shared-files
     ```
   - Commit the submodule addition:
     ```bash
     git commit -m "Add shared-files as submodule in working"
     ```

#### 4. **Sync Changes Across Branches**:
   - After committing changes to the `shared-files` submodule in any branch, you need to **sync** those changes in other branches.
   - To pull the latest changes in the submodule:
     ```bash
     git submodule update --remote
     ```

   - If any branch is ahead with local commits, push the changes:
     ```bash
     git push origin <branch-name>
     ```

---

### **Additional Considerations and Tricky Points**

1. **Handling Duplicate Files:**
   - If files already exist in the branch where you're adding the submodule, you **must remove them** using `git rm` before adding the submodule. Otherwise, you risk having duplicate files and conflict issues.

2. **Reusing an Existing Submodule (`--force` flag):**
   - If you encounter the error: 
     ```bash
     fatal: A git directory for 'shared-files' is found locally...
     ```
     This happens because Git detects that the submodule already exists locally. To reuse the existing directory, add the `--force` flag:
     ```bash
     git submodule add -b shared-files --force https://github.com/your-username/your-repo.git shared-files
     ```

3. **Pushing Submodule Changes to Remote:**
   - After making changes in the submodule, remember to **push** the changes:
     ```bash
     git push origin shared-files
     ```
   - You also need to push the changes in the parent branch (e.g., `master`, `working`):
     ```bash
     git push origin master
     ```

4. **Removing Untracked Local Files:**
   - Even if files are removed from Git tracking (`git rm`), they will still exist in your working directory. You need to **manually delete** them from your system if you don’t need them.

---

### **Commands Summary**:

- **Create a branch**:  
  ```bash
  git checkout -b shared-files
  ```

- **Remove files from tracking**:  
  ```bash
  git rm AI_assist/future_comments.txt AI_assist/prompts.md README.md
  ```

- **Add submodule**:  
  ```bash
  git submodule add -b shared-files https://github.com/your-username/your-repo.git shared-files
  ```

- **Force reuse of existing submodule**:  
  ```bash
  git submodule add -b shared-files --force https://github.com/your-username/your-repo.git shared-files
  ```

- **Update submodule**:  
  ```bash
  git submodule update --remote
  ```

- **Push changes**:  
  ```bash
  git push origin master
  git push origin shared-files
  ```

---

### **End Result**:
- You now have a submodule (`shared-files`) in `master`, `working`, and any future branches that can share the same files across branches.
- Changes made to these files in any branch will be automatically reflected in all other branches when you update the submodule.

Let me know if you need more tweaks or additional details for the cheat sheet!
