# Troubleshooting Guide

This document outlines common issues that may occur while interacting with the GitHub API and how they are handled in the notebook.

---

## 1. Invalid or Missing Token

If the user provides an invalid GitHub Personal Access Token (PAT), or no token at all:

### Error Message

 `401 Unaunthorized`

### How it's handled?

- The function `validate_token()` attempts a test call to `/user`
- If the token is invalid or missing, it prints a warning and falls back to public (unauthenticated) mode
- The rest of the script continues using the public API limit (60 requests/hour)

---

## 2. Rate Limit Reached

GitHub applies rate limits depending on authentication:

| Mode              | Limit                         |
|-------------------|-------------------------------|
| Unauthenticated   | 60 requests per hour per IP   |
| Authenticated     | 5,000 requests per hour       |
| Search endpoints  | 30 requests per minute        |

### Error Message

`403 Forbidden`
`X-RateLimit-Remaining: 0`

### How it is handled?

- In `github_get()`, the code checks if `X-RateLimit-Remaining` is `0`
- If so, it reads the `X-RateLimit-Reset` timestamp
- It waits (`time.sleep()`) until the reset time, then retries the request

This prevents crashes during loops or batch data extractions.

---

## 3. Pagination Not Supported or Unexpected

Some endpoints (like `/contents`) do **not** support pagination. Others like `/commits` do.

### Potential issue

Passing pagination parameters to an endpoint that doesn’t support them could lead to:

- Unexpected data structures (e.g., single dict instead of list)
- Errors like `'dict' object has no attribute 'items'`

### How it's handled?

- Pagination is only enabled when appropriate via a `paginate=True` flag
- If `paginate=False`, page and per_page parameters are excluded
- The code also detects whether the response is a single object, a list, or wrapped in an `items` field

---

## 4. Empty or Unexpected Responses

Some endpoints may return:

- `null` or empty responses
- Single objects instead of lists
- Status 200 but unuseful content

### How it's handled?

- The code checks for `None` or empty `data` in each response
- If the response is not a list, it’s wrapped as a one-item list for safe processing
- Messages like `⚠️ Warning: Empty response` are printed to help identify these issues

---

## 5. Unhandled API Errors

### Other possible responses

- `404 Not Found` (invalid repo or path)
- `422 Unprocessable Entity`
- `500 Internal Server Error`

### How it's handled?

- `github_get()` uses `try/except` around the request logic
- It logs the `status_code` and `response.text`
- If the error is unexpected, it prints a general error and continues

---

## Summary

The code is designed to be resilient against common GitHub API issues by:

- Validating tokens and falling back safely
- Handling rate limits and retrying
- Using dynamic pagination control
- Safeguarding against empty or malformed data
- Capturing unexpected errors without breaking execution
