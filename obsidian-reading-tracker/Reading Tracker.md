---
tags: [dashboard, reading]
---
# 📚 Reading Tracker

> [!tip] How to use
> 1. Create a new note in the `Books/` folder.
> 2. Insert the **Book Template** (`Templates/Book Template.md`) via *Templates: Insert template*.
> 3. Fill in the properties at the top. Change `status` as you go: `to-read` → `reading` → `finished` (or `abandoned`).
>
> The tables below need the **Dataview** community plugin.

## 📖 Currently reading
```dataview
TABLE WITHOUT ID
  file.link AS "Book",
  author AS "Author",
  current-page + " / " + pages AS "Progress",
  round((current-page / pages) * 100) + "%" AS "%",
  started AS "Started"
FROM "Books"
WHERE status = "reading"
SORT started DESC
```

## 🗂️ Up next (to read)
```dataview
TABLE WITHOUT ID
  file.link AS "Book",
  author AS "Author",
  genre AS "Genre",
  pages AS "Pages"
FROM "Books"
WHERE status = "to-read"
SORT file.ctime ASC
```

## ✅ Finished this year
```dataview
TABLE WITHOUT ID
  file.link AS "Book",
  author AS "Author",
  rating AS "Rating",
  finished AS "Finished",
  (finished - started).days + " days" AS "Took"
FROM "Books"
WHERE status = "finished" AND finished.year = date(today).year
SORT finished DESC
```

## 📊 Stats
```dataview
TABLE WITHOUT ID
  length(rows) AS "Books",
  sum(rows.pages) AS "Pages read"
FROM "Books"
WHERE status = "finished"
GROUP BY finished.year AS "Year"
SORT Year DESC
```

## ⭐ Top rated
```dataview
TABLE WITHOUT ID
  file.link AS "Book",
  author AS "Author",
  rating AS "Rating"
FROM "Books"
WHERE status = "finished" AND rating >= 4
SORT rating DESC
LIMIT 10
```

## 💬 Favourite quotes
```dataview
LIST WITHOUT ID item.text + " — " + file.link
FROM "Books"
FLATTEN file.lists AS item
WHERE contains(item.tags, "#quote")
```

## 🪦 Abandoned
```dataview
LIST author
FROM "Books"
WHERE status = "abandoned"
```
