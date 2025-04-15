---
dg-publish: true
tags: ["#module", "#university", stats]
Course Code:
  - "[[STA237]]"
obsidianUIMode: preview
Date created: Mon., Oct. 28, 2024, 4:39:37 pm
Date modified: Sun., Nov. 24, 2024, 5:13:52 pm
---

# Lecture/Notes

```dataviewjs
const pages = dv.pages('(#lecture or #note) and -"Templates"')
    .filter(page => (page["Course Code"]?.join("") ?? "").includes(dv.current()["Course Code"] ?? "")
                    && (page.module?.join("") ?? "").includes(dv.current().file.name ?? "")
            );

const rowsWithChapters = pages.filter(f => f.chapter !== undefined && f.chapter !== null);
const includeChapter = rowsWithChapters.length >= pages.length / 2;

dv.table(
    ['Lecture/Note', 'Date'],
    pages
        .sort((a, b) => {
            // Helper function to check if filename starts with "Week" and extract the number
            const getWeekNumber = (filename) => {
                const match = filename.match(/^Week (\d+)/);
                return match ? parseInt(match[1]) : -1;
            };

            if (includeChapter) {
                const chapterA = a.chapter ? Number(a.chapter) : 0;
                const chapterB = b.chapter ? Number(b.chapter) : 0;
                if (chapterA === chapterB) {
                    const dateA = a.date ? new Date(a.date) : new Date(a.file?.cday || 0);
                    const dateB = b.date ? new Date(b.date) : new Date(b.file?.cday || 0);
                    if (dateA.getTime() === dateB.getTime()) {
                        // Check for Week files as tiebreaker
                        const weekA = getWeekNumber(a.file.name);
                        const weekB = getWeekNumber(b.file.name);
                        if (weekA >= 0 && weekB >= 0) return weekA - weekB;
                        if (weekA >= 0) return -1;
                        if (weekB >= 0) return 1;
                        return a.file.name.localeCompare(b.file.name);
                    }
                    return dateA - dateB;
                }
                return chapterA - chapterB;
            }

            const dateA = a.date ? new Date(a.date) : new Date(a.file?.cday || 0);
            const dateB = b.date ? new Date(b.date) : new Date(b.file?.cday || 0);
            if (dateA.getTime() === dateB.getTime()) {
                // Check for Week files as tiebreaker
                const weekA = getWeekNumber(a.file.name);
                const weekB = getWeekNumber(b.file.name);
                if (weekA >= 0 && weekB >= 0) return weekA - weekB;
                if (weekA >= 0) return -1;
                if (weekB >= 0) return 1;
                return a.file.name.localeCompare(b.file.name);
            }
            return dateA - dateB;
        })
        .map(f => {
            let lecturePrefix = '';
            if (f.lecture) {
                const lectures = Array.isArray(f.lecture) ? f.lecture : [f.lecture];
                const numericLectures = lectures.map(Number).filter(n => !isNaN(n));
                if (numericLectures.length > 0) {
                    const min = Math.min(...numericLectures);
                    const max = Math.max(...numericLectures);
                    lecturePrefix = min === max ? `${min}` : `${min}-${max}`;
                }
            }
            
            const chapterStr = f.chapter ? `${f.chapter}` : '—';
            const tabsNeeded = includeChapter ? (chapterStr.length > 3 ? 2 : 3) : 0;
            const chapterPrefix = includeChapter ? `${chapterStr}${'\t'.repeat(tabsNeeded)}` : '';
            
            const displayLink = lecturePrefix ? `${chapterPrefix} \`L${lecturePrefix}\` ${f.file.link}` : `${chapterPrefix}${f.file.link}`;
            return [displayLink, f.date ? f.date : (f.file?.cday || '')];
        })
);
```
