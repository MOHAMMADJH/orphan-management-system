# دليل ربط Cloudflare R2 Storage

## معلومات R2 المتاحة
- **Bucket Name:** aziton-abo-nageb-r2
- **Location:** Eastern Europe (EEUR)
- **S3 API Endpoint:** https://460e175cd2df5aad68bd7ffde99e1252.r2.cloudflarestorage.com
- **Public Development URL:** https://pub-67ffc00cf89b4300ae00bfe8bb843a8a.r2.dev
- **Created:** Oct 12, 2025

---

## خطوات الإعداد الكاملة

### 1️⃣ الحصول على API Keys من Cloudflare

للحصول على Access Key ID و Secret Access Key:

1. افتح [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. اذهب إلى **R2** من القائمة الجانبية
3. اضغط على **Manage R2 API Tokens**
4. اضغط على **Create API Token**
5. اختر الصلاحيات:
   - **Object Read & Write** للـ bucket: `aziton-abo-nageb-r2`
6. احفظ:
   - **Access Key ID**
   - **Secret Access Key**

⚠️ **مهم جداً:** احفظ Secret Access Key لأنه لن يظهر مرة أخرى!

---

### 2️⃣ تحديث ملف `.env`

بعد الحصول على API Keys، قم بتحديث ملف `.env` في مجلد `backend`:

```env
SECRET_KEY=django-insecure-sayzc-6o(6#9=2g0!vb4*92h1ks*qee&t2n*8k*(-tt)k^hrh3
DEBUG=True

# Database Configuration (Supabase PostgreSQL)
USE_POSTGRES=True
DB_NAME=postgres
DB_USER=postgres
DB_PASSWORD=aziton-abo-nageb-db
DB_HOST=db.yqxuazhduwtkcsbeqmxn.supabase.co
DB_PORT=5432

# File Storage Configuration (Cloudflare R2)
USE_R2_STORAGE=True
AWS_ACCESS_KEY_ID=ضع_هنا_Access_Key_ID
AWS_SECRET_ACCESS_KEY=ضع_هنا_Secret_Access_Key
AWS_STORAGE_BUCKET_NAME=aziton-abo-nageb-r2
AWS_S3_ENDPOINT_URL=https://460e175cd2df5aad68bd7ffde99e1252.r2.cloudflarestorage.com
AWS_S3_CUSTOM_DOMAIN=pub-67ffc00cf89b4300ae00bfe8bb843a8a.r2.dev
AWS_S3_REGION_NAME=eeur
```

---

### 3️⃣ اختبار الإعداد

```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
./venv/bin/python manage.py shell
```

ثم في Shell:
```python
from django.core.files.base import ContentFile
from django.core.files.storage import default_storage

# اختبار رفع ملف
test_content = ContentFile(b"Test file content")
file_path = default_storage.save('test.txt', test_content)
print(f"File uploaded: {file_path}")

# اختبار قراءة الملف
if default_storage.exists(file_path):
    print("File exists!")
    
# اختبار حذف الملف
default_storage.delete(file_path)
print("File deleted!")
```

---

## المزايا

### ✅ مع R2 Storage (موصى به للإنتاج)
- ✨ **تخزين غير محدود** تقريباً
- 🚀 **سريع وآمن**
- 💰 **مجاني** حتى 10GB شهرياً
- 🌍 **CDN عالمي** من Cloudflare
- 🔒 **احتياطي تلقائي**
- 📊 **لا تفقد البيانات** عند إعادة نشر المشروع

### ⚠️ بدون R2 Storage (التخزين المحلي)
- 📁 الملفات تُحفظ في مجلد `media/`
- ⚠️ الملفات قد **تُحذف** عند إعادة نشر المشروع
- 💾 محدود بمساحة السيرفر
- 🐌 أبطأ للمستخدمين البعيدين

---

## الإعدادات المتاحة

### للتطوير المحلي (بدون R2):
```env
USE_R2_STORAGE=False
```
الملفات ستُحفظ في مجلد `backend/media/`

### للإنتاج (مع R2):
```env
USE_R2_STORAGE=True
# + إضافة API Keys
```

---

## حل المشاكل

### خطأ: "NoCredentialsError"
- تأكد من صحة `AWS_ACCESS_KEY_ID` و `AWS_SECRET_ACCESS_KEY`
- تحقق من عدم وجود مسافات زائدة في الملف `.env`

### خطأ: "AccessDenied"
- تأكد من صلاحيات API Token
- يجب أن تكون: **Object Read & Write**

### خطأ: "Could not connect to the endpoint URL"
- تحقق من `AWS_S3_ENDPOINT_URL`
- تأكد من اتصالك بالإنترنت

### الملفات لا تظهر
- تحقق من `AWS_S3_CUSTOM_DOMAIN`
- تأكد من تفعيل Public Development URL في Cloudflare

---

## روابط مفيدة

- 🔗 [Cloudflare R2 Dashboard](https://dash.cloudflare.com/?to=/:account/r2)
- 📖 [Cloudflare R2 Documentation](https://developers.cloudflare.com/r2/)
- 🎯 [Django Storages Documentation](https://django-storages.readthedocs.io/)

---

## ملاحظات مهمة

1. **Public Development URL** محدود ولا يُنصح به للإنتاج
   - للإنتاج: اربط Custom Domain بالـ bucket

2. **الأسعار** (مجاني حتى):
   - 10 GB تخزين شهرياً
   - 1 مليون Class A operations
   - 10 مليون Class B operations

3. **الأمان**:
   - لا تشارك API Keys مع أحد
   - لا ترفع ملف `.env` إلى Git
   - استخدم `.gitignore` دائماً

4. **Migration من التخزين المحلي**:
   - الملفات الموجودة في `media/` لن تُنقل تلقائياً
   - قم برفعها يدوياً إلى R2 إذا لزم الأمر

