# Publish this project on GitHub

The ZIP contains a ready-to-upload repository folder. It is not yet a live GitHub repository because a new remote repository must be created under the account and assigned a visibility setting.

## Option 1 — Upload through the GitHub website

1. Extract `Group8_SMS_Spam_Detection_GitHub_Repository.zip`.
2. Sign in to GitHub and select **New repository**.
3. Use the repository name `Group8_SMS_Spam_Detection`.
4. Choose **Private** unless the instructor explicitly requires public access. A private repository can be shared by inviting the instructor as a collaborator.
5. Create the repository **without** adding a README, `.gitignore`, or license (these files are already included in this project folder).
6. Open the new empty repository, select **Add file → Upload files**, and upload the extracted project files and folders.
7. Commit the upload to the `main` branch.
8. Copy the repository URL and share it with the instructor. For a private repository, first make sure the instructor has access.

## Option 2 — Push using Git

After extracting the ZIP and creating an empty GitHub repository, open a terminal in the extracted project folder and run:

```bash
git init -b main
git add .
git commit -m "Initial Group 8 SMS spam detection submission"
git remote add origin https://github.com/YOUR-USERNAME/Group8_SMS_Spam_Detection.git
git push -u origin main
```

Replace `YOUR-USERNAME` with the GitHub username that owns the new repository.

## Before sharing

- Confirm the instructor can open the repository.
- Keep the notebook and its output cells visible.
- Add the final approved report, presentation slides, and course appendix if your instructor expects them in GitHub.
- Review whether student identifiers in the notebook should remain visible if the repository is made public.