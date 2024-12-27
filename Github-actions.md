# Using GitHub Features: A Beginner's Guide

## **1. Pull Requests**

A pull request is a way to propose changes to a project so others can review and merge them into the main branch.

### Steps:

1. **Fork and Clone the Repository**:

   - Fork the project repository to your GitHub account.
   - Clone it to your computer:
     ```bash
     git clone https://github.com/your-username/repo-name.git
     ```

2. **Create a New Branch**:

   - Use a branch for your changes to keep the main branch clean:
     ```bash
     git checkout -b feature-branch-name
     ```

3. **Make Changes and Commit**:

   - Edit your files, then stage and commit your changes:
     ```bash
     git add .
     git commit -m "Brief description of changes"
     ```

4. **Push Changes to GitHub**:

   - Push your branch to your GitHub fork:
     ```bash
     git push origin feature-branch-name
     ```

5. **Open a Pull Request**:

   - Go to the original repository on GitHub.
   - Click **Pull Requests > New Pull Request**.
   - Select your branch and submit the pull request.

---

## **2. Issue Tracking**

Use issues to track bugs, new features, or discussions about the project.

### Steps:

1. **Go to the Issues Tab**:

   - On the GitHub repository page, click **Issues**.

2. **Create a New Issue**:

   - Click **New Issue**.
   - Add a descriptive **Title** and detailed **Description**.
   - Use **Labels** (e.g., bug, enhancement) to categorize the issue.

3. **Assign Team Members**:

   - Assign the issue to contributors by clicking **Assignees**.

4. **Track Progress**:

   - Add comments to discuss updates.
   - Close the issue once resolved.

---

## **3. Team Management**

Organize and manage contributors in your repository.

### Steps:

1. **Add Collaborators**:

   - Go to **Settings > Collaborators and Teams** in the repository.
   - Invite users by their GitHub username and assign roles:
     - **Read**: View only.
     - **Write**: Push and pull changes.
     - **Admin**: Full access.

2. **Create Teams** (For Organizations):

   - Go to your organization’s page.
   - Click **Teams > New Team**.
   - Add members and assign repositories.

---

## **4. CI/CD Integrations**

Automate testing and deployment using GitHub Actions.

### Steps:

1. **Set Up GitHub Actions**:

   - Go to the repository on GitHub.
   - Click the **Actions** tab and select a prebuilt workflow template (e.g., Node.js, Python).

2. **Create a Workflow File**:

   - GitHub Actions use YAML files stored in `.github/workflows/`.
   - Example: Simple Python CI Workflow:
     ```yaml
     name: Python CI

     on: [push, pull_request]

     jobs:
       build:
         runs-on: ubuntu-latest

         steps:
         - uses: actions/checkout@v2
         - name: Set up Python
           uses: actions/setup-python@v2
           with:
             python-version: 3.8
         - name: Install dependencies
           run: |
             python -m pip install --upgrade pip
             pip install -r requirements.txt
         - name: Run tests
           run: pytest
     ```

3. **Commit and Push the Workflow File**:

   - Add the file to your project and push it:
     ```bash
     git add .github/workflows/main.yml
     git commit -m "Add CI workflow"
     git push
     ```

4. **Monitor Workflow Runs**:

   - Go to the **Actions** tab to see the status of the automation.

---

## **5. Git Commands for Actions**

Use these commands to manage Git actions locally:

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/username/repository.git
   ```

2. **Check Branches**:

   - List all branches:

     ```bash
     git branch -a
     ```

   - Switch to a specific branch:

     ```bash
     git checkout branch-name
     ```

3.

   - **Sync with Remote Repository:**



     Fetch changes:



     git fetch origin



     Pull the latest updates:

     ```bash
     git pull origin branch-name
     ```

4. **Resolve Merge Conflicts**:

   - Open conflicting files and fix conflicts manually.
   - Stage resolved files:
     ```bash
     git add conflicting-file.txt
     ```
   - Commit the merge:
     ```bash
     git commit -m "Resolved merge conflicts"
     ```

With these steps and commands, you can fully utilize GitHub for collaboration and automation.

