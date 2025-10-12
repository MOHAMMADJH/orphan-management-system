# دليل سريع لنشر Frontend على Netlify

## الخطوات الأساسية

### 1. التحضير
✅ تم إنشاء ملف `netlify.toml` في مجلد frontend
✅ تم تحديث `package.json` لدعم Netlify

### 2. رفع على Git

```bash
cd /home/mohjas/CascadeProjects/AboNaheb

# إذا لم يكن Git مهيأ بعد
git init
git add .
git commit -m "Setup for Netlify deployment"

# ربط مع GitHub (غير YOUR_REPO_URL برابط repository الخاص بك)
git remote add origin YOUR_REPO_URL
git push -u origin main
```

### 3. النشر على Netlify

#### الطريقة السهلة (من الموقع):

1. **اذهب إلى**: https://app.netlify.com
2. **اضغط**: "Add new site" → "Import an existing project"
3. **اختر**: GitHub/GitLab/Bitbucket
4. **اختر**: الـ repository الخاص بك
5. **إعدادات البناء**:
   ```
   Base directory: frontend
   Build command: npm run build
   Publish directory: frontend/.next
   ```
6. **متغيرات البيئة**:
   - اذهب لـ Site settings → Environment variables
   - أضف:
     - Key: `NEXT_PUBLIC_API_URL`
     - Value: `https://your-backend-url.com/api`
7. **نشر**: اضغط "Deploy site"

### 4. إعداد Backend للسماح بـ Netlify

بعد الحصول على رابط Netlify (مثل: `https://your-site.netlify.app`)، 
حدّث إعدادات CORS في Backend:

```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
```

افتح `orphan_management/settings.py` وأضف رابط Netlify إلى `CORS_ALLOWED_ORIGINS`:

```python
CORS_ALLOWED_ORIGINS = [
    'http://localhost:3000',
    'https://your-site.netlify.app',  # أضف رابطك هنا
]
```

ثم ارفع Backend من جديد.

### 5. التحقق

- افتح الموقع: `https://your-site.netlify.app`
- جرب تسجيل الدخول
- أضف بيانات
- تأكد من عمل جميع الوظائف

## ملاحظات مهمة

- ⏱️ أول build قد يأخذ 3-5 دقائق
- 🔄 التحديثات المستقبلية ستُنشر تلقائياً عند push إلى Git
- 🌐 الموقع سيعمل على HTTPS تلقائياً
- 💰 الخطة المجانية توفر 300 build minutes شهرياً

## إذا واجهتك مشاكل

راجع الدليل الكامل في: `NETLIFY_DEPLOYMENT.md`

## الأوامر المفيدة

```bash
# استخدام Netlify CLI (اختياري)
npm install -g netlify-cli
netlify login
cd frontend
netlify init
netlify deploy --prod
```

