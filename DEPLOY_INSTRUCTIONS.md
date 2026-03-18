# מדריך פריסה ל-Vercel + PlanetScale

## שלב 1: הכנת Database (PlanetScale)

1. היכנס ל-https://planetscale.com/
2. צור חשבון חינמי
3. צור database חדש (תן לו שם כמו `maron-ai-db`)
4. לחץ על "Connect" וקבל את פרטי החיבור
5. שמור את ה-DATABASE_URL (נראה כמו: `mysql://user:pass@aws.connect.psdb.cloud/dbname?sslaccept=strict`)

## שלב 2: פריסה ל-Vercel

### דרך 1: דרך האתר (מומלץ למתחילים)

1. היכנס ל-https://vercel.com/
2. צור חשבון (אפשר להתחבר עם GitHub)
3. לחץ על "Add New Project"
4. העלה את התיקייה `maron_ai` (לחץ על "Browse" ובחר את כל הקבצים)
5. Vercel יזהה אוטומטית שזה פרויקט Vite
6. **חשוב:** לפני Deploy, הוסף את משתני הסביבה:
   - `DATABASE_URL` - מה שקיבלת מ-PlanetScale
   - `NODE_ENV` = `production`
7. לחץ על "Deploy"

### דרך 2: דרך Vercel CLI (למתקדמים)

```bash
# התקן את Vercel CLI
npm install -g vercel

# התחבר לחשבון
vercel login

# פרוס את הפרויקט
cd maron_ai
vercel

# עקוב אחרי ההוראות במסך
# כשישאל על build settings, אשר את ברירות המחדל
```

## שלב 3: הגדרת משתני סביבה ב-Vercel

אם לא הוספת אותם בשלב 2:

1. לך לדף הפרויקט ב-Vercel
2. Settings → Environment Variables
3. הוסף:
   - `DATABASE_URL` - מה שקיבלת מ-PlanetScale
   - כל משתני סביבה אחרים שצריכים להיות

## שלב 4: הרצת Migrations

אחרי שהפרויקט עולה, תצטרך להריץ את ה-database migrations:

```bash
# מהמחשב שלך
pnpm install
DATABASE_URL="<הURL_שלך>" pnpm run db:push
```

## בעיות נפוצות

### שגיאת "Cannot connect to database"
- ודא ש-DATABASE_URL נכון
- ודא שה-IP של Vercel מורשה ב-PlanetScale (בדרך כלל אוטומטי)

### האתר לא נטען (404)
- ודא ש-build הצליח
- בדוק Logs ב-Vercel

### Functions timeout
- Vercel מגביל functions ל-10 שניות (חינמי)
- אם יש queries ארוכים, צריך לייעל

## קישורים שימושיים

- Vercel Dashboard: https://vercel.com/dashboard
- PlanetScale Dashboard: https://app.planetscale.com/
- Vercel Docs: https://vercel.com/docs

---

## הערה חשובה על העברה מ-Express ל-Vercel

הפרויקט שלך משתמש ב-Express server מלא. Vercel עובד עם Serverless Functions.
זה אומר שתצטרך להמיר את ה-routes מ-Express ל-Vercel Functions.

אם תרצה עזרה בהמרה, תן לי לדעת!
