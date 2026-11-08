# Free deployment (Vercel + Render + Neon)

## 1) Database (Neon)
1. Neon par free project banayein.
2. SQL Editor mein `database/schema.sql` ka poora content run karein.
3. Neon ka pooled connection string copy karein. `?sslmode=require` rehne dein.

## 2) API (Render)
1. ZIP ko GitHub repository mein upload/push karein.
2. Render par **New > Blueprint** se repository connect karein; `render.yaml` detect hoga.
3. `DATABASE_URL` mein Neon connection string paste karein.
4. `FIELD_ENCRYPTION_KEY` mein 64 hexadecimal characters (32 random bytes) paste karein. Example generate: `openssl rand -hex 32`.
5. `CORS_ORIGIN` mein Vercel frontend URL set karein (example `https://your-app.vercel.app`).
6. Deploy ke baad API URL note karein, e.g. `https://nursecare-pro-api.onrender.com`.

## 3) Frontend (Vercel)
1. Vercel par same GitHub repository import karein.
2. Framework preset **Other** rehne dein; `vercel.json` build set karta hai.
3. Environment Variable add karein: `VITE_API_URL` = Render API URL (without trailing slash).
4. Deploy karein.

## 4) First administrator
Render service ke Environment tab se `BOOTSTRAP_SECRET` ki generated value copy karein. API URL ke saath yeh request run karein (password 12+ characters):

```bash
curl -X POST https://YOUR-API.onrender.com/api/auth/register \
  -H 'Content-Type: application/json' \
  -H 'x-bootstrap-secret: YOUR-BOOTSTRAP-SECRET' \
  -d '{"username":"admin","password":"CHANGE_THIS_LONG_PASSWORD","fullName":"Hospital Admin","role":"nurse_admin"}'
```

Phir website par us username/password se login karein. Registration endpoint bootstrap secret ke baghair blocked hai.

## Notes
- Render free services sleep after inactivity; first request may take a minute.
- Neon/Vercel/Render free quotas and policies can change.
- `FIELD_ENCRYPTION_KEY`, `JWT_SECRET`, `BOOTSTRAP_SECRET`, and DB URL private rakhein; GitHub par commit na karein.
- Yeh development starter hai, certified clinical/EHR software nahi. Real patient data use na karein jab tak proper security, privacy, backup, and clinical approvals na hon.
