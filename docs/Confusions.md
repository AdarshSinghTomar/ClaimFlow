Confusions 
usecase of gitkeep when its empty
## Step 1 — Git lessons from real errors (2026-09-25)
- **Did:** Created the GitHub repo with a README + .gitignore, then built a
  separate local repo. The first push failed ("src refspec main does not
  match any") because no commit/branch named main existed locally. Fixed by
  committing and renaming the branch to main, then force-pushing over
  GitHub's auto-generated commit.
- **Learned:** Local and remote histories are separate; push only works when
  mine extends GitHub's. A branch exists only after its first commit.
  Start from one side only (empty remote + push, OR clone). --force replaces
  remote history, so it's only safe when the remote has nothing worth keeping.
  PowerShell and Command Prompt use different file commands.
- **Next time:** Create the GitHub repo empty, or clone it first. Run
  git status and git branch before trying fixes.