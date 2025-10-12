# نشر المشروع على Vercel 🚀

## أيتام شرق الزيتون

### 📋 متطلبات قبل النشر

#### 1. Backend (Django API)
يجب نشر الـ Backend أولاً على خدمة مثل:
- Render.com
- Railway.app
- DigitalOcean
- Heroku

وستحتاج إلى:
- قاعدة بيانات PostgreSQL
- تخزين للملفات (Cloudflare R2 أو AWS S3)

#### 2. تجهيز الـ Backend
```bash
cd backend
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

---

## 🌐 خطوات النشر على Vercel

### الخطوة 1: تثبيت Vercel CLI (اختياري)
```bash
npm i -g vercel
```

### الخطوة 2: تسجيل الدخول
```bash
vercel login
```

### الخطوة 3: رفع المشروع
انتقل إلى مجلد Frontend:
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/frontend
vercel
```

أو استخدم واجهة Vercel:
1. اذهب إلى https://vercel.com
2. انقر على "Add New Project"
3. اختر GitHub/GitLab أو ارفع المشروع مباشرة
4. اختر مجلد `frontend`

---

## ⚙️ متغيرات البيئة (Environment Variables)

في لوحة تحكم Vercel، أضف المتغيرات التالية:

### Required Variables:
```
NEXT_PUBLIC_API_URL=https://your-backend-api-url.com/api
```

مثال:
```
NEXT_PUBLIC_API_URL=https://abonaheb-api.onrender.com/api
```

---

## 🔧 إعدادات Build

### Build Command:
```bash
npm run build
```

### Output Directory:
```
.next
```

### Install Command:
```bash
npm install
```

### Framework Preset:
```
Next.js
```

---

## 📝 ملاحظات مهمة

### 1. تحديث CORS في Backend
في `backend/orphan_management/settings.py`:
```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://127.0.0.1:3000",
    "https://your-vercel-app.vercel.app",  # أضف رابط Vercel
]
```

### 2. تحديث ALLOWED_HOSTS
```python
ALLOWED_HOSTS = [
    'localhost',
    '127.0.0.1',
    'your-backend-domain.com',
]
```

### 3. Static Files
تأكد من تكوين تخزين الملفات (R2/S3) في Backend

---

## 🎯 الأوامر المفيدة

### بناء محلي للاختبار:
```bash
cd frontend
npm run build
npm start
```

### نشر جديد على Vercel:
```bash
vercel --prod
```

### فحص الأخطاء:
```bash
npm run build
```

---

## ✅ التحقق من النشر

بعد النشر الناجح:
1. ✅ افتح الرابط الذي توفره Vercel
2. ✅ تحقق من صفحة Landing Page
3. ✅ جرب تسجيل الدخول (الإدارة)
4. ✅ تحقق من اتصال API

---

## 🐛 حل المشاكل الشائعة

### المشكلة: API لا يعمل
**الحل:** تحقق من:
- متغير `NEXT_PUBLIC_API_URL` في Vercel
- إعدادات CORS في Backend
- Backend يعمل بشكل صحيح

### المشكلة: الصور لا تظهر
**الحل:** تحقق من:
- تكوين R2/S3 في Backend
- `remotePatterns` في `next.config.ts`

### المشكلة: الخطوط العربية لا تظهر
**الحل:** تحقق من:
- استيراد Cairo font في `layout.tsx`
- Tailwind CSS configuration

---

## 📞 الدعم

للمساعدة أو الاستفسارات:
- البريد الإلكتروني: info@eastzeitoun.org
- الهاتف: +970 123 456 789

---

## 🎉 تهانينا!

مشروعك الآن على الإنترنت! 
زر الموقع: https://your-project.vercel.app

---

**ملاحظة:** تأكد من تحديث جميع الروابط والمتغيرات بالقيم الفعلية لمشروعك.

