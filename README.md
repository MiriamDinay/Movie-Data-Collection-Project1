# 🎬 Movie Data Collection - Project Part 1

## פרויקט סופי - קורס כרייה וניתוח נתונים מתקדם בפייתון

זהו החלק הראשון מתוך שלושה של פרויקט הגמר בקורס. מטרת חלק זה היא בניית סט נתונים מקיף ואיכותי של נתוני סרטים אשר ישמש בחלקים הבאים של הפרויקט: בניית מודלי חיזוי (חלק 2) ופיתוח אפליקציית Flask (חלק 3).

## 👥 צוות הפרויקט

| שם | ת.ז. |
|---|---|
| מרים דינאי | 212296099 |
| הילה הלברשטט | 032540668 |

## 📂 מבנה הפרויקט

```
Movie-Data-Collection-Project1/
├── notebook.ipynb         # מחברת הקוד הראשית
├── dataset.csv            # סט הנתונים הסופי (5,000 סרטים)
├── report.pdf             # דוח מסכם
└── README.md              # קובץ זה
```

## 📊 על סט הנתונים

סט הנתונים מכיל **5,000 סרטים** שכותרתם מתחילה באות **L** (האות שהוקצתה לצוות שלנו על ידי המרצה).

### שדות סט הנתונים (13 עמודות):

| שם השדה | תיאור | מקור |
|---|---|---|
| `tconst` | מזהה ייחודי של הסרט ב-IMDb | IMDb |
| `primaryTitle` | שם הסרט | IMDb |
| `startYear` | שנת יציאה לאקרנים | IMDb |
| `genres` | רשימת ז'אנרים | IMDb |
| `lead_actors_ids` | רשימת 5 השחקנים המובילים | IMDb |
| `runtimeMinutes` | אורך הסרט בדקות | IMDb |
| `averageRating` | ממוצע דירוג IMDb | IMDb |
| `numVotes` | מספר ההצבעות | IMDb |
| `Language` | השפה הראשית | Wikipedia |
| `Country` | מדינת ההפקה | Wikipedia |
| `budget` | תקציב במיליוני USD | Wikipedia |
| `BoxOffice` | הכנסות במיליוני USD | Wikipedia |
| `plot` | תקציר עלילה | Wikipedia |

### פילטרים שיושמו:

- ✅ `titleType == 'movie'` - סרטים בלבד (ללא סדרות)
- ✅ `60 ≤ runtimeMinutes ≤ 300` - אורך סרט סטנדרטי
- ✅ `startYear ≤ 2024` - לפי דרישות המטלה
- ✅ `primaryTitle` מתחיל באות L

## 🛠️ דרישות מערכת והתקנה

### גרסת פייתון
Python 3.8 ומעלה (פותח על Python 3.13)

### ספריות נדרשות

```bash
pip install pandas requests beautifulsoup4 tqdm CurrencyConverter
```

או דרך `requirements.txt` (אם קיים):

```bash
pip install -r requirements.txt
```

### ספריות בשימוש

| ספרייה | מטרה |
|---|---|
| `pandas` | טיפול ב-DataFrames וקבצי TSV/CSV |
| `requests` | שליחת בקשות HTTP לוויקיפדיה |
| `BeautifulSoup` | פירוק HTML של דפי ויקיפדיה |
| `re` | ביטויים רגולריים לחילוץ נתונים |
| `tqdm` | מד התקדמות ויזואלי |
| `CurrencyConverter` | המרת מטבעות לדולרים לפי שנה |
| `os`, `time` | ניהול קבצים ו-rate limiting |

## 📥 קבצי קלט נדרשים

לפני הרצת הקוד, יש להוריד את הקבצים הבאים מ-IMDb ולשמור אותם באותה תיקייה כמו המחברת:

מקור: https://developer.imdb.com/non-commercial-datasets/

- `title.basics.tsv` (מתוך `title.basics.tsv.gz`)
- `title.ratings.tsv` (מתוך `title.ratings.tsv.gz`)
- `title.principals.tsv` (מתוך `title.principals.tsv.gz`)

> ⚠️ **שימו לב:** קובץ `title.principals.tsv` גדול מאוד (כמה GB) - הקוד מטפל בו באמצעות קריאה ב-chunks כדי למנוע בעיות זיכרון.

## 🚀 הרצת הקוד

```bash
jupyter notebook notebook.ipynb
```

הריצו את התאים בסדר. הזרימה הכללית:

1. **טעינה וסינון של נתוני IMDb** - טעינת `title.basics.tsv` וסינון לסרטי L
2. **מיזוג נתוני דירוג** - הוספת `averageRating` ו-`numVotes`
3. **שליפת שחקנים** - קריאת `title.principals.tsv` ב-chunks ושליפת 5 השחקנים המובילים
4. **בחירת 5,000 הסרטים העליונים** - לפי `numVotes` (לא אקראית)
5. **Web Scraping מויקיפדיה** - חילוץ Language, Country, budget, BoxOffice, plot
6. **שמירת הסט הסופי** - ל-`dataset.csv`

> ⏱️ **זמן ריצה משוער:** כ-30-35 דקות (בעיקר שלב ה-scraping). בהרצה חוזרת, מערכת ה-cache מקצרת זאת משמעותית.

## 🔍 שיטת האיסוף

### 1. שלב ה-IMDb
טעינת קבצי TSV באמצעות `pandas` וסינון לפי הקריטריונים שנקבעו במטלה. עבור קובץ `title.principals.tsv` הגדול, השתמשנו בקריאה ב-chunks של 200,000 שורות כדי למנוע overflow של זיכרון.

### 2. בחירת הסרטים ל-Scraping
החלטנו **לא לבחור 5,000 סרטים אקראית**, אלא לבחור את 5,000 הסרטים בעלי **מספר ההצבעות הגבוה ביותר** - מתוך הסרטים שאין בהם ערכים חסרים בעמודות מ-IMDb. הסיבה: סרטים פופולריים יותר נוטים להופיע בויקיפדיה עם דף מלא, מה שמשפר משמעותית את שיעור הצלחת ה-scraping ואיכות הסט הסופי.

### 3. Web Scraping מויקיפדיה
- **אסטרטגיית Multi-URL:** עבור כל סרט ניסינו רצף של תבניות URL אפשריות (`Title_(YEAR_film)`, `Title_(film)`, וכד') כדי למקסם את שיעור ההצלחה.
- **מערכת Cache מקומית:** כל דף שהורד נשמר בתיקיית `wiki_pages/`, כך שניתן להריץ את הקוד מחדש מבלי להוריד שוב.
- **Rate Limiting:** הוספנו `time.sleep(0.5)` בין בקשות ו-`User-Agent` תקין כדי לציית לפרקטיקות scraping אתי.
- **Backup אוטומטי:** כל 100 סרטים נשמר גיבוי ל-`dataset_backup.csv` במקרה של קריסה.

### 4. עיבוד נתונים פיננסיים
פונקציית `parse_wiki_money` מטפלת באתגרי הנתונים הפיננסיים:
- ניקוי הערות שוליים (כגון `[1]`)
- זיהוי מטבעות שונים (`$`, `£`, `€`, `¥`, `₹`, `francs`, `lit.` וכו')
- טיפול בטווחים (חישוב ממוצע)
- המרת `billion` למיליונים
- המרה לדולרים אמריקאיים לפי שער החליפין בשנת יציאת הסרט

## 📈 סטטיסטיקת ערכים חסרים

| עמודה | % חסרים | מקור |
|---|---|---|
| כל עמודות IMDb (8) | 0.00% | IMDb |
| Language | 41.06% | Wikipedia |
| Country | 41.42% | Wikipedia |
| plot | 49.24% | Wikipedia |
| BoxOffice | 78.20% | Wikipedia |
| budget | 84.48% | Wikipedia |

> 📝 הסבר מלא לערכים החסרים מופיע בדוח (`report.pdf`).

## 📑 תיעוד נוסף

- **דוח מלא:** ראו `report.pdf` לפירוט מלא של ההחלטות, השיטות והניתוחים.
- **מחברת הקוד:** `notebook.ipynb` מכילה הסברים inline בכל שלב של התהליך.

## ⚖️ שימוש הוגן

הפרויקט עושה שימוש במאגרי נתונים ציבוריים בלבד:
- **IMDb Non-Commercial Datasets:** בהתאם לתנאי השימוש הציבוריים של IMDb.
- **Wikipedia:** Web Scraping מתון ומכבד, עם השהיות בין בקשות ו-cache מקומי כדי להפחית עומס.

## 📜 רישיון

פרויקט זה הוגש כחלק מקורס אקדמי. השימוש בנתונים הוא לצרכי לימוד בלבד.

---

**קישור ל-GitHub:** [Movie-Data-Collection-Project1](https://github.com/MiriamDinay/Movie-Data-Collection-Project1)
