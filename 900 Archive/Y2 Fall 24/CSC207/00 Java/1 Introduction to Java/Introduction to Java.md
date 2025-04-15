---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', chapter, university]
Course Code:
  - '[[CSC207]]'
Week: 1
Module:
  - '[[Java]]'
Date: 2024-09-07
Chapter: 1
Date created: Sat., Sep. 7, 2024, 6:00:21 pm
Date modified: Wed., Oct. 30, 2024, 5:51:49 pm
---

> [!question] How do we do `print(7 + 5)` in Java?

```dataviewjs
const currentFilePath = dv.current().file.path;
const currentCourseCode = dv.current()["Course Code"].join("") || "";
const pages = dv.pages('(#lecture or #note) and -"Templates"').filter(p => {
    const pageCourseCode = p["Course Code"]?.join("") || "";
    return p.file.path !== currentFilePath
        && pageCourseCode.includes(currentCourseCode)
        && (p.chapter ? Math.floor(p.chapter) === Math.floor(dv.current().chapter) : false);
    }
).sort(p => p.chapter);

dv.table([], pages.map(p => [p.chapter, p.file.link, p.date ? p.date : p.file.cday]));
```
