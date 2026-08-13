# 💎 PRO-FINANCE — מערכת לניהול פיננסי אישי

> אפליקציית ווב בעברית (RTL) לניהול תקציב משק בית, שנבנתה מאפס כקובץ HTML יחיד — ללא שרת, ללא framework, וללא שליחת נתונים לשום מקום.

**[▶ להרצת הדגמה חיה](https://shovalnaor.github.io/Pro-Finance-Demo/index.html)** · Vanilla JS · Local-First · Hebrew RTL

---

## על הפרויקט

התחלתי את הפרויקט מבעיה אישית: כדי להבין לאן הכסף שלי הולך בכל חודש הייתי צריך לקפוץ בין אפליקציות של שני בנקים, שתי חברות אשראי וגיליון אקסל. האפליקציות המסחריות שבדקתי היו יקרות, לא באמת תמכו בעברית, או דרשו למסור פרטי התחברות לבנק.

הדרישה המרכזית שהגדרתי לעצמי הייתה **לדעת שאני עומד לחרוג מהתקציב לפני שזה קורה**, ולא אחרי שמגיע פירוט האשראי. מכאן נגזרה כל הארכיטקטורה: מודל חיזוי שמפריד הוצאות קבועות ממשתנות, מנוע התראות תקציב, וזיהוי אנומליות בזמן אמת.

המערכת פותחה במהלך חודשים של שימוש יומיומי ואיטרציות, והגרסה שכאן היא **גרסת הדגמה לתיק עבודות** — עם נתוני דמו מיוצרים, ללא שום מידע פיננסי אמיתי.

---

## יכולות עיקריות

### ניתוח וחיזוי
| יכולת | תיאור |
|---|---|
| **מדד בריאות פיננסית** | ציון 0–100 המורכב מארבעה רכיבים משוקללים: שיעור חיסכון (35%), עמידה בתקציב (30%), יציבות חודשית (20%) ויחס הוצאות קבועות להכנסה (15%) |
| **מודל חיזוי הוצאות** | הפרדה בין הוצאות קבועות (הוראות קבע — נכנסות ב-100%) לבין הוצאות משתנות (מוחלקות לפי קצב יומי), עם ניכוי אנומליות חד-פעמיות והצגת רמת מהימנות לפי כמות ההיסטוריה הקיימת |
| **זיהוי אנומליות** | סימון אוטומטי של עסקאות החורגות פי 3 מהממוצע ההיסטורי בקטגוריה |
| **ניתוח פארטו** | זיהוי ריכוזיות — מתי שתי קטגוריות בלבד אחראיות ליותר מ-70% מההוצאה החודשית |
| **זיהוי דפוסים** | השוואת קצב ההוצאה בסופי שבוע מול אמצע השבוע |

### תפעול יומיומי
- **ייבוא חכם מכרטיסי אשראי** — זיהוי אוטומטי של מבנה הקובץ (MAX / ישראכארט / כאל) לפי שמות עמודות נרדפים, מיפוי קטגוריות חברת האשראי לקטגוריות המערכת, זיהוי כפילויות, ומסך אישור לפני הכנסה למסד
- **למידת סיווג מההיסטוריה** — המערכת זוכרת איך סיווגת בית עסק בעבר ומיישמת את אותו סיווג אוטומטית
- **הוראות קבע** — מנוע שמזין תנועות חוזרות אוטומטית בכל חודש, עם הגנה מפני כפילות
- **פריסת תשלומים** — פיצול אוטומטי של עסקה למספר חודשים
- **תגיות חופשיות** — חיתוך נתונים בחתך שאינו קטגוריה (למשל "חופשה", "רכב חדש")

### תצוגה ודיווח
מעקב תקציב חודשי לכל קטגוריה עם התראת חריגה · גרף Waterfall של זרימת כספים · מפת חום של הוצאות יומיות · פילוח יומי מוערם לפי קטגוריה · יעדי חיסכון עם חישוב צפי השלמה לפי קצב החיסכון בפועל · סימולטור ריבית דריבית · ייצוא דוח PDF · ייצוא/שחזור גיבוי JSON · מצב כהה · עברית/אנגלית

### AI Advisor (אופציונלי)
אינטגרציה ל-Gemini API שמקבלת את המדדים המחושבים והחיזויים כקונטקסט. ה-prompt מנוסח במפורש לביקורתיות — לתת משוב אובייקטיבי ולא מרצה. **המפתח מוזן על ידי המשתמש ונשמר בדפדפן בלבד**; הפיצ'ר עובד גם בלעדיו (שאר המערכת אינה תלויה בו).

---

## ארכיטקטורה ושיקולי תכנון

```
index.html  ←  קובץ יחיד: HTML + CSS + JS  (~2,000 שורות)
```

**Local-First במכוון.** כל הנתונים נשמרים ב-`localStorage` של הדפדפן ולא עוזבים את המכשיר. אין backend, אין בסיס נתונים, אין הרשמה. זו לא הייתה מגבלה טכנית אלא החלטת מוצר: כשמדובר בנתונים פיננסיים אישיים, "אין שרת" היא הבטחת הפרטיות החזקה ביותר שאפשר לתת.

**ללא framework.** Vanilla JS בלבד. הפרויקט קטן מספיק כדי שהתקורה של React לא תשתלם, והאילוץ הזה שמר על זמן טעינה מיידי ועל שליטה מלאה בשכבת ה-RTL.

**מקור אמת יחיד לסיווג תנועות.** הפונקציה `getTxType()` היא הנקודה היחידה שקובעת אם תנועה היא הכנסה, הוצאה או העברה (השקעה/חיסכון). ההפרדה הזו חשובה: העברה לתיק השקעות אינה הוצאה, ולכן היא לא מקטינה את היתרה אלא נספרת כחיסכון בחישוב שיעור החיסכון.

### מחסנית טכנולוגית
`HTML5` · `CSS3 (Custom Properties, Grid, Flexbox)` · `Vanilla JavaScript (ES6+)` · `Chart.js` + `annotation plugin` · `SheetJS (xlsx)` · `localStorage` · `Gemini API`

---

## הרצה מקומית

```bash
git clone https://github.com/Shovalnaor/Pro-Finance-Demo.git
cd Pro-Finance-Demo
```

פתחו את `index.html` בדפדפן. זהו — אין תלויות להתקין ואין תהליך build.

בטעינה הראשונה נטענים אוטומטית נתוני הדגמה. ניתן לאפס ולטעון אותם מחדש בכל שלב דרך **הגדרות ← טען נתוני הדגמה מחדש**.

---

## מגבלות ידועות

הפרויקט נבנה לצורך אישי ולתיק עבודות, ולכן יש דברים שהוא מודע להם ולא פותר:

- **אין סנכרון בין מכשירים** — נגזרת ישירה של ארכיטקטורת Local-First. נתונים עוברים בין מכשירים רק דרך ייצוא/ייבוא קובץ גיבוי.
- **`localStorage` מוגבל בנפח** (בערך 5MB) ונמחק עם ניקוי נתוני הדפדפן. גיבוי תקופתי הוא באחריות המשתמש.
- **ייבוא האשראי מכסה את הפורמטים שנתקלתי בהם בפועל** — פורמט של חברה אחרת ידרוש הרחבה של מילון העמודות.
- **החיזוי הוא סטטיסטי ולא סיבתי** — הוא מבוסס על ממוצעים היסטוריים, ולכן רגיש לשינויים חדים בהתנהלות.

---

## רישיון

הקוד מפורסם לצפייה ולהתרשמות בלבד. כל הזכויות שמורות — ראו [LICENSE](LICENSE).

---

## יצירת קשר

**שובל נאור** · בוגר הנדסת תעשייה וניהול (B.Sc.), התמחות במערכות מידע — שנקר

📧 shovaln1310@gmail.com · 💼 [LinkedIn](https://linkedin.com/in/shoval-naor) · 💻 [GitHub](https://github.com/Shovalnaor)

---

<details>
<summary><b>English Summary</b></summary>

<br>

**PRO-FINANCE** is a Hebrew RTL personal finance web application, built from scratch as a single HTML file — no server, no framework, no data leaving the device.

**The problem:** I wanted to know I was about to exceed my monthly budget *before* it happened, not after the credit card statement arrived. Existing budgeting apps were expensive, had poor Hebrew support, or required handing over bank credentials.

**Key capabilities**
- Composite financial health score (0–100) weighing savings rate, budget adherence, month-over-month stability, and fixed-cost ratio
- Spend forecasting that separates fixed recurring costs from variable spend, strips one-off anomalies, and reports its own confidence level based on available history
- Automatic anomaly detection (transactions 3× above category average), Pareto concentration analysis, and weekday/weekend pattern detection
- Smart credit card statement import with automatic format detection across Israeli issuers, category mapping, duplicate detection, and a confirmation step
- Category learning from historical classifications
- Recurring payment engine, installment splitting, savings goals with completion projections, compound interest simulator, PDF reporting, JSON backup/restore, dark mode, Hebrew/English i18n
- Optional Gemini API integration for critical (deliberately non-flattering) financial feedback — user-supplied key, stored locally

**Stack:** Vanilla JavaScript (ES6+), Chart.js, SheetJS, CSS Custom Properties, localStorage.

**Architecture note:** the local-first design is a product decision rather than a technical constraint — for personal financial data, having no server at all is the strongest privacy guarantee available.

**License:** proprietary — source published for portfolio review only. See [LICENSE](LICENSE).

</details>
