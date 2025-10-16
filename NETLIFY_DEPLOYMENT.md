# دليل نشر Frontend على Netlify

## المتطلبات الأساسية
- حساب على [Netlify](https://www.netlify.com)
- الكود موجود على Git repository (GitHub, GitLab, أو Bitbucket)
- Backend مرفوع ومشغل

## الخطوات

### 1. تحضير المشروع

#### أ. تحديث package.json (اختياري)
إذا أردت فصل أوامر البناء للتطوير والإنتاج:

```json
"scripts": {
  "dev": "next dev --turbopack",
  "build": "next build",
  "build:turbo": "next build --turbopack",
  "start": "next start"
}
```

ملاحظة: Netlify يفضل استخدام `next build` بدون `--turbopack` لتوافق أفضل.

### 2. رفع الكود على Git Repository

```bash
cd /home/mohjas/CascadeProjects/AboNaheb

# إذا لم يكن Git مهيأ
git init

# إضافة remote repository
git remote add origin YOUR_REPOSITORY_URL

# إضافة الملفات
git add .

# عمل commit
git commit -m "Prepare frontend for Netlify deployment"

# رفع على GitHub/GitLab
git push -u origin main
```

### 3. إعداد Netlify

#### الطريقة الأولى: من خلال موقع Netlify (مُفضلة)

1. **تسجيل الدخول إلى Netlify**
   - اذهب إلى https://app.netlify.com
   - سجل دخول أو أنشئ حساب جديد

2. **إضافة موقع جديد**
   - اضغط على "Add new site" → "Import an existing project"
   - اختر Git provider (GitHub/GitLab/Bitbucket)
   - اختر الـ repository الخاص بك

3. **إعدادات البناء (Build Settings)**
   ```
   Base directory: frontend
   Build command: npm run build
   Publish directory: frontend/.next
   ```

4. **متغيرات البيئة (Environment Variables)**
   - اذهب إلى Site settings → Environment variables
   - أضف:
     ```
     NEXT_PUBLIC_API_URL = https://your-backend-url.com/api
     ```

5. **تثبيت Next.js Plugin**
   - اذهب إلى Plugins
   - ابحث عن "@netlify/plugin-nextjs"
   - اضغط Install

6. **نشر الموقع**
   - اضغط "Deploy site"
   - انتظر حتى ينتهي البناء

#### الطريقة الثانية: استخدام Netlify CLI

```bash
# تثبيت Netlify CLI
npm install -g netlify-cli

# تسجيل الدخول
netlify login

# الانتقال لمجلد frontend
cd /home/mohjas/CascadeProjects/AboNaheb/frontend

# تهيئة Netlify
netlify init

# اتبع التعليمات واختر:
# - Create & configure a new site
# - Team: اختر team الخاص بك
# - Site name: اختر اسم للموقع
# - Build command: npm run build
# - Publish directory: .next

# بناء ونشر
netlify deploy --prod
```

### 4. إعدادات إضافية

#### أ. Custom Domain
1. اذهب إلى Domain settings
2. Add custom domain
3. اتبع تعليمات إضافة DNS records

#### ب. HTTPS
- Netlify يوفر HTTPS تلقائياً
- يمكنك تفعيل Force HTTPS من Domain settings

#### ج. البناء التلقائي
- افتراضياً، Netlify يبني الموقع تلقائياً عند كل push
- يمكنك تعطيل ذلك من Build settings إذا أردت

### 5. التحقق من النشر

بعد النشر الناجح:

1. **افتح الموقع**
   - استخدم URL المؤقت: `https://your-site-name.netlify.app`

2. **اختبر الوظائف**
   - تسجيل الدخول
   - إضافة يتيم
   - عرض البيانات
   - الـ PWA features

3. **تحقق من Console**
   - افتح Developer Tools (F12)
   - تأكد من عدم وجود أخطاء في Console
   - تحقق من أن API calls تعمل بشكل صحيح

### 6. معالجة المشاكل الشائعة

#### مشكلة: Build Failed

**الحل:**
```bash
# تحديث dependencies
cd frontend
npm install

# تجربة البناء محلياً
npm run build

# إذا نجح محلياً، تحقق من:
# - Node version على Netlify (يجب أن يكون 20)
# - Environment variables مضافة بشكل صحيح
```

#### مشكلة: API calls لا تعمل

**الحل:**
- تحقق من `NEXT_PUBLIC_API_URL` في Environment variables
- تأكد من أن Backend يسمح بـ CORS من domain الجديد:
  ```python
  # في backend/orphan_management/settings.py
  CORS_ALLOWED_ORIGINS = [
      'https://your-site.netlify.app',
  ]
  ```

#### مشكلة: Images لا تظهر

**الحل:**
- تحقق من إعدادات R2 في Backend
- تأكد من أن R2 bucket له public access
- تحقق من `next.config.ts` أن domains مضافة بشكل صحيح

#### مشكلة: PWA لا يعمل

**الحل:**
- تأكد من أن الموقع يعمل على HTTPS
- تحقق من أن `manifest.json` و `sw.js` موجودين في `/public`
- افتح DevTools → Application → Service Workers

### 7. تحديثات مستقبلية

عند عمل تحديثات جديدة:

```bash
# في مجلد frontend
git add .
git commit -m "وصف التحديث"
git push origin main

# Netlify سيبني ويرفع تلقائياً
```

### 8. إلغاء Vercel (اختياري)

إذا أردت حذف تكوينات Vercel:

```bash
cd /home/mohjas/CascadeProjects/AboNaheb/frontend
rm vercel.json
```

### 9. Rollback (التراجع عن نسخة)

إذا حدثت مشكلة في نشر معين:

1. اذهب إلى Deploys في Netlify Dashboard
2. اختر النشر السابق الذي يعمل
3. اضغط "Publish deploy"

## ملاحظات مهمة

1. **الـ Base Directory**: تأكد من تعيين `frontend` كـ base directory في Netlify
2. **Environment Variables**: يجب أن تبدأ بـ `NEXT_PUBLIC_` لتكون متاحة في client-side
3. **Build Time**: أول build قد يأخذ 3-5 دقائق
4. **الحد المجاني**: Netlify يوفر 300 build minutes شهرياً في الخطة المجانية
5. **CDN**: Netlify يستخدم CDN عالمي لتسريع الموقع

## موارد إضافية

- [Netlify Next.js Documentation](https://docs.netlify.com/frameworks/next-js/overview/)
- [Netlify Environment Variables](https://docs.netlify.com/environment-variables/overview/)
- [Netlify Build Settings](https://docs.netlify.com/configure-builds/overview/)

## الدعم

إذا واجهت مشاكل:
1. تحقق من Build logs في Netlify Dashboard
2. راجع Function logs إذا كان هناك مشاكل في runtime
3. استخدم Netlify Support إذا كانت المشكلة من جانبهم



