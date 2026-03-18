# התחלה מהירה - Vercel Deployment

## 📦 מה יש בתיקייה הזו?

✅ **vercel.json** - הגדרות פריסה ל-Vercel
✅ **DEPLOY_INSTRUCTIONS.md** - הוראות מלאות לפריסה
✅ **MIGRATION_GUIDE.md** - מדריך המרה מפורט
✅ **.env.example** - דוגמה למשתני סביבה

## 🚀 התחלה מהירה (5 דקות)

### שלב 1: PlanetScale (Database חינמי)
1. לך ל-https://planetscale.com/
2. צור חשבון → Create Database
3. שמור את ה-DATABASE_URL

### שלב 2: Vercel (Hosting חינמי)
1. לך ל-https://vercel.com/
2. התחבר עם GitHub (או Email)
3. New Project → Import
4. גרור את התיקייה `maron_ai` או העלה את הזיפ
5. **לפני Deploy:** הוסף Environment Variable:
   - Key: `DATABASE_URL`
   - Value: מה שקיבלת מ-PlanetScale
6. Deploy!

### שלב 3: אחרי הפריסה
```bash
# הרץ migrations (מהמחשב שלך)
cd maron_ai
pnpm install
DATABASE_URL="<הURL שלך>" pnpm run db:push
```

## ⚠️ חשוב לדעת

הפרויקט משתמש ב-**Express server** אבל Vercel זה **Serverless**.

אם האתר לא עובד מיד, תצטרך לבצע **המרה** (ראה MIGRATION_GUIDE.md).

### 3 אפשרויות:

1. **פריסה מהירה (Frontend בלבד)** - עובד מיד, אבל ללא backend
2. **Adapter** - השתמש ב-@vercel/node adapter (קל, עובד סביר)
3. **המרה מלאה** - המר ל-Vercel Functions (הכי מהיר, צריך עבודה)

## 💰 עלויות

- **Vercel**: חינמי לגמרי (Hobby plan)
- **PlanetScale**: חינמי עד 5GB
- **סה"כ**: 0₪/חודש ✨

## 📚 קישורים

- [הוראות פריסה מלאות](DEPLOY_INSTRUCTIONS.md)
- [מדריך המרה](MIGRATION_GUIDE.md)
- [Vercel Dashboard](https://vercel.com/dashboard)
- [PlanetScale Dashboard](https://app.planetscale.com/)

---

**זקוק לעזרה?** פתח את DEPLOY_INSTRUCTIONS.md לפרטים מלאים!
