# A11Y-DISCOVERY.md — נוף בחצר | בדיקת נגישות בסיסית

## מיפוי קבצים

```
site/
├── index.html          ← עמוד ראשי יחיד (791 שורות, כולל CSS ו-JS inline)
├── hero-video.mp4      ← וידאו רקע ה-Hero
└── images/
    ├── logo.png
    ├── hero-poster.jpg
    ├── about.jpg
    ├── wine.jpg
    ├── food-1.jpg, food-2.jpg, food-3.jpg
    └── gallery-1.jpg … gallery-7.jpg
```

אין קובצי CSS/JS חיצוניים — הכל inline ב-`index.html`.

---

## מיפוי אלמנטים

| רכיב | Selector / ID | הערות |
|---|---|---|
| וידאו Hero | `video.hero-media` (source: hero-video.mp4) | autoplay muted loop playsinline, poster="images/hero-poster.jpg" |
| פופ-אפ גיוס | `#hiring-popup` | display:flex position:fixed — נפתח בטעינה, נסגר בלחיצת ✕ |
| קרוסלת גלריה | `#carouselTrack` + `.carousel-btn` | 7 שקופיות, 3 גלויות בבת אחת (desktop), 1 במובייל |
| תפריט עליון | `nav.top-nav` בתוך `header.top-bar` | נעלם ב-<900px (display:none), אין המבורגר |
| פוטר | `footer` | קישורי רשתות חברתיות, טלפון, וואטסאפ, Waze |
| לוגו (nav) | `a.logo[href="#"]` | מצביע על "#" — בעיית נגישות |

---

## טבלת צבעים ובדיקת ניגודיות

| משתנה | HEX | שימוש |
|---|---|---|
| `--beige-light` | `#faf6ee` | רקע ראשי (about, body) |
| `--beige` | `#f4ede0` | רקע גלריה, tags |
| `--beige-dark` | `#e8dcc4` | פוטר רקע, border-dark |
| `--white` | `#ffffff` | רקע wine-kitchen, info |
| `--green` | `#4a5d3f` | כפתורים, section-labels |
| `--green-dark` | `#3a4a32` | כותרות h3, tag text |
| `--text` | `#2d2d2a` | טקסט ראשי |
| `--text-soft` | `#5a5a55` | טקסט משני |
| `--line` | `#d8cdb4` | קווי הפרדה (דקורטיבי) |

### בדיקות ניגודיות (WebAIM WCAG 2.0 AA)

| צמד | יחס ניגודיות | דרישה | תוצאה |
|---|---|---|---|
| `--text #2d2d2a` על `--beige-light #faf6ee` | ~12.5:1 | ≥4.5:1 | ✅ עובר |
| `--text-soft #5a5a55` על `--beige-light #faf6ee` | ~6.4:1 | ≥4.5:1 | ✅ עובר |
| `--text-soft #5a5a55` על `--white #ffffff` | ~7.0:1 | ≥4.5:1 | ✅ עובר |
| `--green #4a5d3f` על `--beige-light #faf6ee` | ~6.4:1 | ≥4.5:1 | ✅ עובר (section-labels) |
| `--green-dark #3a4a32` על `--beige #f4ede0` | ~9.1:1 | ≥4.5:1 | ✅ עובר (tag) |
| `--white #fff` על `--green #4a5d3f` | ~6.4:1 | ≥4.5:1 | ✅ עובר (כפתורים) |
| `--white #fff` על `--green-dark #3a4a32` | ~9.1:1 | ≥4.5:1 | ✅ עובר (final-cta) |
| טקסט Hero (לבן) על וידאו | תלוי פריים | ≥4.5:1 | ⚠️ overlay rgba(0,0,0,.4) — גבולי, הוגדל ל-.55 |
| `--line #d8cdb4` (גבולות) | — | דקורטיבי | ✅ פטור |
| פוקוס outline `#8a6d0b` על `--beige-light #faf6ee` | ~4.6:1 | ≥3:1 | ✅ עובר |

**אין "זהב על קרם" בפלטה הנוכחית** — הסיכון היה בפלטות קודמות; הפלטה הנוכחית עוברת AA בכל הצמדים.

---

## ליקויים שנמצאו (Baseline)

### קריטי
- [ ] אין `<main>` landmark — קורא מסך לא יכול לנווט לתוכן
- [ ] אין קישור "דלג לתוכן" — משתמשי מקלדת חייבים לעבור כל ה-nav בכל טעינה
- [ ] פופ-אפ גיוס: אין `role="dialog"`, אין `aria-modal`, אין מלכודת פוקוס, אין סגירה ב-Esc
- [ ] וידאו רץ ללא כפתור עצירה — WCAG 2.2.2 (Pause, Stop, Hide)

### חמור
- [ ] `a.logo[href="#"]` — קישור ריק ללא יעד משמעותי
- [ ] אין הצהרת נגישות (חובה חוקית ת"י 5568)
- [ ] אין רכיב נגישות
- [ ] מלכודת פוקוס חסרה במודאל
- [ ] כותרת H2 בפופ-אפ מופיעה לפני H1 ב-DOM (מבלבל קורא מסך)

### בינוני
- [ ] `outline:none` / `outline:0` לא נמצא, אך אין `focus-visible` מוגדר ברמת הגלובל
- [ ] כפתורי קרוסלה: יש `aria-label` בסיסי אך מיקום RTL לא מוסבר
- [ ] תמונות בגלריה: יש alt אבל חלקן ריקות או גנריות

---

## ציון Baseline (Lighthouse Accessibility)
*נדרש ריצה ידנית — משוער 55-65 בהתבסס על הליקויים*

---

## ציון סופי (לאחר תיקונים)
*לעדכן אחרי הרצת Lighthouse סופית*
