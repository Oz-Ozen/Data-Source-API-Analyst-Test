
# 📊 Data Source API Analyst Test

This repository contains the solution to the technical assigment for the **Data Source API Analyst** role.

All task were completed using Python and Google Colab, following step by step instructions in the test.

---

## Purpose of each task

### 1. **Understand Client Needs**

The client requieres assistence to access 3 endpoints/objetives via GitHub's API REST:

- Search Publics repositories
- Access commits from a repo
- read a repo (file/folder)from a repo

---

### 2. **Research GitHub's API**

We reviewed the GitHubs's API to understand:

- How authenticate via token
- Pagination Logic
- Rate limit (primary and secondary)
- How to handles some errors (401, 403)

Enpoints Used:

- `GET /search/repositories`
- `GET /repos/{owner}/{repo}/commits`
- `GET /repos/{owner}/{repo}/contents/{path}`

(See [api_reference.md](Content/api_reference.md) for details.)
  
---

### 3. **Google Colab**

- Core logic is implemented in `Content/git_hub_colab.ipynb`
- Includes reusable functions `github_get()` that handles:
  - Authentication
  - Rate limits
  - Pagination
  - Error management
- Wrapper functions are provided for:
  - Searching repositories
  - Getting commits
  - Getting file contents
- Bonus: Extra helpers to filter repos, and download files or entires repos as a zip.

---

### 4. **Troubleshooting guide**

Covers common issues like:

- Missed or invalid token
- Empty responses
- Hitting GitHub's rate limit

(See [troubleshooting.md](Content/troubleshooting.md) for detailed guide.)

---

### 5. **Get result and reflections**

- All work is documented in the [Colab notebook](Content/github_api_colab.ipynb)
- Suppoting markdown files are located in the `Content/` folder.
- Postman was not used; Google Colab was chosen for clarity and reproducibility

---

### **Folder Structure**

📁 Content/

├── github_api_colab.ipynb ← Colab notebook with all test logic

├── api_reference.md ← Endpoints used and their purpose

├── troubleshooting.md← Common issues and handling

├── data_cleaning.md ← How API responses were cleaned and filtered

├── download_utilities.md ←  File and repo download functions

📁 Postman_Collection/

└── (Empty – Colab was used instead of Postman)
