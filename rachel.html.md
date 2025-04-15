---
dg-publish: true
tags: []
aliases: []
dg-home: true
obsidianUIMode: preview
Date created: Tue., Oct. 17, 2023, 1:57:36 pm
Date modified: Wed., Jan. 22, 2025, 12:00:39 am
---

# 2024 - 2025 Fall/Winter

```dataview
TABLE WITHOUT ID
file.link AS "Courses",
choice(Quercus, "[" + row["Course Name"] + "]" + "(" + Quercus + ")", row["Course Name"]) AS "",
string(Semester) AS "Semester"
FROM #course-page
WHERE contains(Semester, "W25")
SORT string(Semester), file.name
```
