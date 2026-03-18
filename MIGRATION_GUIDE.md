# המרת הפרויקט ל-Vercel - מדריך מפורט

## הבעיה העיקרית

הפרויקט שלך בנוי כ-Express server מלא (server/index.ts), אבל Vercel עובד עם **Serverless Functions**.

## מה זה אומר?

במקום שיהיה לך server אחד שרץ כל הזמן, ב-Vercel כל route הוא function נפרד שמופעל רק כשצריך.

## שתי אפשרויות:

---

## אפשרות 1: פריסה מהירה (רק Frontend)

אם אתה רוצה רק לראות את האתר עובד **ללא Backend**, זה הכי פשוט:

1. פרוס רק את ה-client
2. האתר יעבוד אבל ללא שמירת נתונים/התחברות/וכו'

### איך עושים את זה:

```bash
# ערוך את vercel.json להיות:
{
  "buildCommand": "cd client && npm run build",
  "outputDirectory": "client/dist",
  "framework": "vite"
}
```

**יתרונות:**
- ✅ עובד מיד
- ✅ לא צריך שינויים

**חסרונות:**
- ❌ אין backend
- ❌ אין database
- ❌ רק UI סטטי

---

## אפשרות 2: המרה מלאה ל-Serverless (מומלץ!)

להמיר את ה-Express routes ל-Vercel Functions.

### מבנה הנדרש:

```
/api
  /trpc
    [trpc].ts          ← כל ה-tRPC routes
  /auth
    login.ts           ← login endpoint
    logout.ts          ← logout endpoint
/client
  (הכל כמו שזה)
```

### דוגמה להמרה:

**לפני (Express):**
```typescript
// server/index.ts
app.post('/api/auth/login', async (req, res) => {
  // login logic
});
```

**אחרי (Vercel Function):**
```typescript
// api/auth/login.ts
import type { VercelRequest, VercelResponse } from '@vercel/node';

export default async function handler(
  req: VercelRequest,
  res: VercelResponse
) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }
  
  // login logic (אותו קוד)
  
  return res.status(200).json({ success: true });
}
```

### המרת tRPC:

```typescript
// api/trpc/[trpc].ts
import { createNextApiHandler } from '@trpc/server/adapters/next';
import { appRouter } from '../../server/routers';

export default createNextApiHandler({
  router: appRouter,
  createContext: () => ({}),
});
```

---

## אפשרות 3: שימוש ב-Adapter (הכי קל!)

Vercel תומך ב-Express דרך adapter:

### 1. התקן את ה-adapter:
```bash
pnpm add @vercel/node
```

### 2. צור api/index.ts:
```typescript
import app from '../server/_core/index';

export default app;
```

### 3. עדכן vercel.json:
```json
{
  "rewrites": [
    { "source": "/api/(.*)", "destination": "/api" }
  ]
}
```

**יתרונות:**
- ✅ מינימום שינויים
- ✅ האפליקציה תעבוד כמעט כמו שהיא

**חסרונות:**
- ⚠️ יותר איטי מ-native Vercel Functions
- ⚠️ מגבלות timeout (10 שניות)

---

## המלצה שלי:

1. **התחל עם אפשרות 3** (Adapter) - הכי פשוט
2. אם זה לא עובד טוב, **עבור לאפשרות 2** (המרה מלאה)
3. אם אתה רק רוצה לראות את ה-UI - **אפשרות 1**

## מה תרצה לעשות?

אני יכול לעזור לך עם כל אחת מהאפשרויות!
