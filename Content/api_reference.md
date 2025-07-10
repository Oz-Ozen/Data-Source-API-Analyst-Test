# 📶 GitHub API reference

This document outlines the main GitHub REST API endpoints used in the Data Source API Analyst Test.

All endpoints follow this base url:[`https://api.github.com`](https://api.github.com)

---

## 1. Search Repositories

**Endpoint:** GET /search/repositories

**Full Example:** [`https://api.github.com/search/repositories?q=topic:data-science&per_page=5&page=1`](https://api.github.com/search/repositories?q=topic:data-science&per_page=5&page=1)

**Used for**:

- Searching public repositories by keyowords, topic, or other filters.

**Key parameters:**

- `q` : Search query (e.g. `topic:data`)
- `per_page` : Number of results per page (max 100)
- `page`: Page number

**Special Notes:**

- Subject to [secondary rate limits](https://docs.github.com/en/rest/search?apiVersion=2022-11-28#about-the-search-api)(30 requests/minute per ip)

---

## 2. List Commits

**Endpoint:** GET /repos/{owner}/{repo}/commits

**Full Example:** [`https://api.github.com/repos/pandas-dev/pandas/commits?per_page=100&page=1`](https://api.github.com/repos/pandas-dev/pandas/commits?per_page=100&page=1)

**Used for:**

- Fetching recent commits from a repository

**Key Parameters:**

- `sha`: Optional, branch or commit SHA
- `since`: Date range for commits
- `per_page`, `page`: For pagination

---

## 3. List Repo Contents

**Endpoint:** GET /repos/{owner}/{repo}/contents/{path}

**Full Examples:**

- Root:  [`https://api.github.com/repos/pandas-dev/pandas/contents/`](https://api.github.com/repos/pandas-dev/pandas/contents/)
- Specific File: [`https://api.github.com/repos/pandas-dev/pandas/contents/README.md`](https://api.github.com/repos/pandas-dev/pandas/contents/README.md)

**Used For:**

- Listing files and folders in a repository
- Accessing metadata or download URLs for specific files

**Notes:**

- If the path is a file, returns a JSON object
- If the path is a folder, returns a JSON list of items
- No pagination supported

---

## Headers Used

| Header | Purpose |
|--------|---------|
| `Authorization: Bearer <token>` | Required for authenticated requests |
| `Accept: application/vnd.github+json` | Required for JSON format |
| `X-GitHub-Api-Version: 2022-11-28` | Ensures consistent API behavior |

---

## Pagination Strategy

- For endpoints that support pagination (`search`, `commits`):
- Used `page` and `per_page` query parameters
- Looped until either:
  - `Link: next` header was missing
  - Or response size was less than `per_page`
