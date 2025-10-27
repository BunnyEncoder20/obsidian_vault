# Git

***Can't be a programmer without having a deep knowledge of git*** - *Linus Torvalds

## Creating and Switching to the Feature Branch

You'll use two main Git commands: `git switch` (or the older `git checkout`) and `git branch`.

### 1. **Create and Switch to the New Branch:**

It's best practice to create the new branch directly from your current location (which should be `main` if you just fetched/pulled). 
```bash
git switch -c feature/my-new-feature-name
```
- **`git switch`**: The modern command for switching branches.
- **`-c`**: This flag tells Git to **create** the branch if it doesn't exist and then switch to it immediately.
- **`feature/my-new-feature-name`**: Choose a descriptive name. Using a prefix like `feature/`, `fix/`, or `chore/` is a common convention.

### 2. **Verify Your Location:**
- Make sure you are on the new branch before you start coding:

```bash
git branch
```
- You should see an asterisk (`*`) next to your new branch name, indicating it's the active branch. 

---

## The Workflow Going Forward

Now that you're on your feature branch, here's how the rest of your process works:

 1. **Code:** Make your changes and write your code on this branch.
 2. **Commit:** Commit your changes regularly:
```bash
git add .
git commit -m "feat: Implement the new user profile settings"
```
3. **Push:** Push your local branch to the remote repository (e.g., on GitHub, GitLab, or Bitbucket):
```bash
git push -u origin feature/my-new-feature-name
```
- The `-u` flag sets the upstream tracking, so future simple `git push` commands will know where to send your changes.
 
4. **Create a Pull Request (PR):** Go to your remote repository interface (GitHub, etc.) and create a **Pull Request** from your `feature/my-new-feature-name` branch _into_ the **`main`** branch. 🔗    
5. **Review & Merge:** Once your PR is reviewed, approved, and passes any automated checks, it can be merged into `main`.

### Post-Merge Cleanup (Optional but Recommended)

After your feature branch is successfully merged into `main` and pushed:

1. **Switch back to `main`:**
```bash
git switch main
```    
	
2. **Pull latest changes** (to get the merge commit):
```bash
git pull
```
	
3. **Delete the local branch:**
```bash
git branch -d feature/my-new-feature-name
```
	  
4. **Delete the remote branch** (usually done via the platform's UI after the merge, but can also be done via command line):
```bash
git push origin --delete feature/my-new-feature-name
```
