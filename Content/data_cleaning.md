# Data Cleaning Approach

This project usues GitHub's REST API, which returns nested *JSON* objects with many fields - often more than required.

To clean and simplify the data for the analysis, the following strategies were used:

---

## 1. Commit Data Cleaning

The raw `/commits` endpoints includes deeply nested fields.
We created a helper function to extract onlty the relevant information

```python
{
    "sha": commit.get("sha"),
    "author": commit.get("commit", {}).get("author", {}).get("name"),
    "date": commit.get("commit", {}).get("author", {}).get("date"),
    "message": commit.get("commit", {}).get("message"),
    "url": commit.get("html_url")
}
```

This flattening step makes it easier to:

- Display commit history
- Group commits by author
- Track Commit frequency over time

## 2. Respository Filtering

From the `/search/` repositories endpoint, we filtered results using:

- Minimun star count
- Specific language
- Custom query keywords

This helps remove irrelevant or low-quality repositories from analysis.

---

## Summary

While GitHub's API provides clean JSON, it's often verbose and deeply nested.
This project includes post-processing functions to extract meaningful data for reporting and reusse
