# 🚀 פריסה מהירה ב-Vercel עם Express Adapter

## ✨ מה השתנה?

הוספתי **Express Adapter** שמאפשר לך לפרוס את הפרויקט **כמו שהוא** ללא שינויים!

### הקבצים שנוספו:
- ✅ `api/index.js` - wrapper ל-Express server
- ✅ `client/package.json` - הגדרות build
- ✅ `vercel.json` מעודכן

---

## 📋 שלב אחר שלב

### שלב 1: PlanetScale (Database - 2 דקות)

1. לך ל-https://planetscale.com/
2. **Sign up** (חינמי)
3. **Create database** → תן שם (למשל: `maron-ai-db`)
4. לחץ **Connect** → **Create password**
5. העתק את ה-**Connection string** שמתחיל ב-`mysql://...`
6. שמור אותו בצד!

---

### שלב 2: Vercel (Hosting - 3 דקות)

#### דרך 1: דרך האתר (הכי קל! 👍)

1. לך ל-https://vercel.com/signup
2. **התחבר עם GitHub** (או Email)
3. לחץ **Add New... → Project**
4. לחץ **Browse** → בחר את התיקייה `maron_ai` (אחרי שחילצת את הזיפ)
5. Vercel יזהה את הפרויקט אוטומטית
6. **חשוב מאוד!** לפני Deploy:
   - גלול למטה ל-**Environment Variables**
   - הוסף:
     - **Key**: `DATABASE_URL`
     - **Value**: הדבק את ה-Connection string מ-PlanetScale
     - **Key**: `NODE_ENV`
     - **Value**: `production`
7. לחץ **Deploy** 🎉

#### דרך 2: דרך CLI (למתקדמים)

```bash
# התקן Vercel CLI
npm install -g vercel

# התחבר
vercel login

# פרוס
cd maron_ai
vercel

# הוסף Environment Variables בהמשך דרך Dashboard
```

---

### שלב 3: הרץ Database Migrations (2 דקות)

אחרי שהפרויקט עלה ב-Vercel:

```bash
cd maron_ai
pnpm install
DATABASE_URL="<paste_connection_string_here>" pnpm run db:push
```

---

## 🎯 זהו! האתר שלך אמור לעבוד

הכתובת תהיה משהו כמו:
`https://maron-ai.vercel.app`

---

## ⚙️ איך זה עובד?

הפרויקט משתמש ב-**@vercel/node adapter** שמאפשר ל-Express server שלך לרוץ כ-Serverless Function.

### מבנה:
```
/api/index.js          ← Express server עטוף כ-Vercel Function
/client/               ← Frontend (Vite/React)
/dist/public/          ← Build output של ה-Frontend
```

### Flow:
1. כל request ל-`/api/*` → מנותב ל-Express server
2. כל request אחר → מנותב לקבצים הסטטיים של ה-Frontend

---

## 🔧 בעיות נפוצות

### "Cannot connect to database"
- ✅ ודא ש-DATABASE_URL נכון (העתקת אותו נכון מ-PlanetScale)
- ✅ בדוק ב-Vercel Dashboard → Settings → Environment Variables

### "Build failed"
- ✅ בדוק את ה-logs ב-Vercel Dashboard
- ✅ ודא שה-package.json תקין

### "Function timeout"
- ⚠️ Vercel מגביל functions ל-**10 שניות** (free tier)
- אם יש queries ארוכים, צריך לייעל או לשדרג

### האתר מציג 404
- ✅ ודא שה-build הצליח
- ✅ נסה לגשת ל-`/api/health` (אם יש endpoint כזה)
- ✅ בדוק Routes ב-vercel.json

---

## 💰 כמה זה עולה?

- **Vercel Free (Hobby)**: חינמי לגמרי
  - 100GB Bandwidth
  - Unlimited deploys
  - 10s Function timeout
  
- **PlanetScale Free**: חינמי לגמרי
  - 5GB Storage
  - 1 Billion row reads/month
  - 10 Million row writes/month

**סה"כ: 0₪/חודש!** ✨

---

## 📚 קישורים שימושיים

- [Vercel Dashboard](https://vercel.com/dashboard)
- [PlanetScale Dashboard](https://app.planetscale.com/)
- [Vercel Docs](https://vercel.com/docs)
- [Express in Vercel](https://vercel.com/guides/using-express-with-vercel)

---

## 🆘 צריך עזרה?

אם משהו לא עובד:
1. בדוק את ה-**Logs** ב-Vercel Dashboard
2. ודא ש-Environment Variables הוגדרו
3. נסה להריץ `pnpm run build` מקומית לבדוק שה-build עובד

---

**בהצלחה! 🚀**
