# ✅ قائمة التحقق - نشر على Netlify

## قبل البدء

- [ ] لديك حساب على GitHub/GitLab/Bitbucket
- [ ] لديك حساب على Netlify
- [ ] Backend مرفوع ويعمل
- [ ] لديك رابط Backend API

## خطوات التحضير ✅ (تم إنجازها)

- [x] تم إنشاء ملف `netlify.toml`
- [x] تم تحديث `package.json`
- [x] تم إنشاء `.netlifyignore`
- [x] تم إنشاء الدلائل التوضيحية

## خطوات النشر (افعلها الآن)

### 1. Git Setup
- [ ] رفع الكود على Git repository
  ```bash
  cd /home/mohjas/CascadeProjects/AboNaheb
  git add .
  git commit -m "Setup for Netlify"
  git push origin main
  ```

### 2. Netlify Setup
- [ ] تسجيل الدخول إلى https://app.netlify.com
- [ ] "Add new site" → "Import an existing project"
- [ ] اختيار Git provider والـ repository
- [ ] إعدادات البناء:
  - Base directory: `frontend`
  - Build command: `npm run build`
  - Publish directory: `frontend/.next`

### 3. متغيرات البيئة
- [ ] إضافة `NEXT_PUBLIC_API_URL` في Netlify
  - Site settings → Environment variables
  - Key: `NEXT_PUBLIC_API_URL`
  - Value: `https://your-backend-url.com/api`

### 4. تثبيت Next.js Plugin
- [ ] اذهب إلى Plugins في Netlify
- [ ] ابحث عن "@netlify/plugin-nextjs"
- [ ] اضغط Install

### 5. النشر
- [ ] اضغط "Deploy site"
- [ ] انتظر حتى ينتهي البناء (3-5 دقائق)
- [ ] احصل على رابط الموقع (مثل: `https://your-site.netlify.app`)

### 6. تحديث Backend CORS
- [ ] افتح `/backend/orphan_management/settings.py`
- [ ] أضف رابط Netlify إلى `CORS_ALLOWED_ORIGINS`:
  ```python
  CORS_ALLOWED_ORIGINS = [
      "http://localhost:3000",
      "https://your-site.netlify.app",  # رابطك هنا
  ]
  ```
- [ ] احفظ واعد تشغيل Backend

### 7. الاختبار
- [ ] افتح رابط Netlify
- [ ] جرب تسجيل الدخول
- [ ] أضف بيانات جديدة
- [ ] تحقق من عرض الصور
- [ ] جرب جميع الصفحات

### 8. إعدادات اختيارية
- [ ] إضافة Custom Domain (إذا أردت)
- [ ] تفعيل Force HTTPS
- [ ] تعديل Build settings حسب الحاجة

## ملفات مهمة للمراجعة

- 📄 `NETLIFY_DEPLOYMENT.md` - دليل كامل مفصل
- 📄 `نشر_NETLIFY.md` - دليل سريع بالعربية
- 📄 `backend/NETLIFY_CORS_UPDATE.md` - دليل تحديث CORS
- 📄 `frontend/netlify.toml` - إعدادات Netlify
- 📄 `frontend/env.netlify.example` - مثال لمتغيرات البيئة

## في حالة المشاكل

### Build Failed
- [ ] تحقق من Build logs في Netlify
- [ ] تأكد من Node version = 20
- [ ] تحقق من Environment variables

### API لا يعمل
- [ ] تحقق من `NEXT_PUBLIC_API_URL`
- [ ] تحقق من CORS في Backend
- [ ] افتح Console (F12) وتحقق من الأخطاء

### الصور لا تظهر
- [ ] تحقق من R2 settings
- [ ] تأكد من `next.config.ts` صحيح

## بعد النشر الناجح

- [ ] احفظ رابط الموقع
- [ ] شارك الرابط مع فريقك
- [ ] اختبر على أجهزة مختلفة
- [ ] اختبر على متصفحات مختلفة

## التحديثات المستقبلية

لعمل تحديث جديد:
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/frontend
# عمل التعديلات...
git add .
git commit -m "وصف التحديث"
git push origin main
# Netlify سيبني وينشر تلقائياً!
```

---

**ملاحظة**: إذا واجهت أي مشكلة، راجع الدلائل المفصلة أو افتح Build logs في Netlify Dashboard.



