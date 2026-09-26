# Posting this repo to GitHub

## 1. Add the screenshots

Every challenge folder has an `images/` subfolder with a short `README.md` listing the exact filenames its write-up expects. Open the matching original write-up document, save each screenshot, and drop it into that folder under the listed name. The write-ups already reference those paths, so once the files are in place the images render on GitHub with no edits.

You can post before adding every image. Missing images show as broken links but the text reads fine, so you can fill them in over time.

## 2. Create the repository

On GitHub, click the "+" top right, New repository. Name it something like `finbot-ctf-writeups`, public or private as you prefer, and do not initialise it with a README since this repo already has one.

## 3. Upload

If you use GitHub Desktop (the reliable route for folders full of images):

1. Clone the empty repo to a local folder.
2. Copy the contents of this `finbot-ctf-writeups` folder into it (`README.md`, `POSTING.md`, `.gitignore`, and the `challenges/` folder).
3. In GitHub Desktop, review the change list, write a commit summary like "Add FinBot CTF write-ups," commit to main, then push.

If you use the command line:

```
git init
git add .
git commit -m "Add FinBot CTF write-ups"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/finbot-ctf-writeups.git
git push -u origin main
```

## 4. Check it rendered

Open the repo on GitHub. The front page shows the index table, and each challenge link opens its own write-up. Confirm a couple of the images load once you have added them.

## Adding a new challenge later

Create a new folder under `challenges/` following the `NN-slug` pattern, give it a `README.md` and an `images/` folder, then add one row to the table in the top-level `README.md`.
