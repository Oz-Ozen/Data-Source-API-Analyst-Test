# ⬇️ Download Utilities

This document explains optional helper functions included in the notebook for downloading data directly from GitHub repositories.

---

## 1. Download a Single File

**Function:** `download_file_from_repo(owner, repo, path, save_as=None)`

**How it works:**

- Calls the GitHub `/contents` API
- Extracts the `download_url` field
- Downloads the raw file and saves it locally

**Useful for:**

- Saving README files
- Extracting scripts or configs from public repos

---

## 2. Download an Entire Repository

**Function:** `download_repo_zip(owner, repo, branch="main", save_as=None)`

**How it works:**

- Builds the GitHub `.zip` download URL directly (not via API)
- Downloads the ZIP and saves it locally

**Useful for:**

- Taking a full snapshot of a repo
- Offline analysis

---

## Notes

- These features work without authentication (public repos only)
- They do not affect API rate limits
