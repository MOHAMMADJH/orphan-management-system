# دليل الإعداد الكامل للمشروع
## Supabase PostgreSQL + Cloudflare R2

---

## 📋 نظرة عامة

هذا الدليل يشرح كيفية ربط المشروع مع:
1. **Supabase PostgreSQL** - قاعدة البيانات السحابية
2. **Cloudflare R2** - تخزين الملفات السحابي

---

## 🎯 الخطوات السريعة

### المتطلبات الأساسية
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
./venv/bin/pip install psycopg2-binary==2.9.9
```

### إنشاء ملف `.env`

قم بإنشاء ملف `.env` في مجلد `backend` بالمحتوى التالي:

#### للإنتاج (مع قاعدة البيانات + التخزين السحابي):
```env
SECRET_KEY=django-insecure-sayzc-6o(6#9=2g0!vb4*92h1ks*qee&t2n*8k*(-tt)k^hrh3
DEBUG=True

# Supabase PostgreSQL
USE_POSTGRES=True
DB_NAME=postgres
DB_USER=postgres
DB_PASSWORD=aziton-abo-nageb-db
DB_HOST=db.yqxuazhduwtkcsbeqmxn.supabase.co
DB_PORT=5432

# Cloudflare R2
USE_R2_STORAGE=True
AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY_HERE
AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY_HERE
AWS_STORAGE_BUCKET_NAME=aziton-abo-nageb-r2
AWS_S3_ENDPOINT_URL=https://460e175cd2df5aad68bd7ffde99e1252.r2.cloudflarestorage.com
AWS_S3_CUSTOM_DOMAIN=pub-67ffc00cf89b4300ae00bfe8bb843a8a.r2.dev
AWS_S3_REGION_NAME=eeur
```

#### للتطوير المحلي (بدون خدمات سحابية):
```env
SECRET_KEY=django-insecure-sayzc-6o(6#9=2g0!vb4*92h1ks*qee&t2n*8k*(-tt)k^hrh3
DEBUG=True

USE_POSTGRES=False
USE_R2_STORAGE=False
```

---

## 🔑 الحصول على R2 API Keys

1. افتح [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. اذهب إلى **R2** → **Manage R2 API Tokens**
3. اضغط على **Create API Token**
4. اختر صلاحيات **Object Read & Write** للـ bucket
5. احفظ **Access Key ID** و **Secret Access Key**

⚠️ **مهم:** Secret Access Key يظهر مرة واحدة فقط!

---

## 🚀 تشغيل المشروع

### 1. تطبيق Migrations
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
./venv/bin/python manage.py migrate
```

### 2. إنشاء مستخدم إداري
```bash
./venv/bin/python manage.py createsuperuser
```

### 3. تشغيل Backend
```bash
./venv/bin/python manage.py runserver
```

### 4. تشغيل Frontend (في terminal آخر)
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/frontend
npm run dev
```

---

## 📊 السيناريوهات المختلفة

### السيناريو 1: كل شيء محلي (التطوير)
```env
USE_POSTGRES=False    # SQLite محلي
USE_R2_STORAGE=False  # ملفات في media/
```
✅ لا يحتاج إنترنت
✅ سريع للتطوير
⚠️ البيانات تُحذف بسهولة

### السيناريو 2: قاعدة بيانات سحابية فقط
```env
USE_POSTGRES=True     # Supabase
USE_R2_STORAGE=False  # ملفات في media/
```
✅ بيانات آمنة في السحابة
⚠️ الملفات محلية

### السيناريو 3: كل شيء في السحابة (الإنتاج - موصى به)
```env
USE_POSTGRES=True     # Supabase
USE_R2_STORAGE=True   # Cloudflare R2
```
✅ بيانات آمنة 100%
✅ ملفات سريعة عبر CDN
✅ قابل للتوسع
⚠️ يحتاج إنترنت

---

## 🧪 اختبار الإعداد

### اختبار قاعدة البيانات
```bash
./venv/bin/python manage.py check --database default
```

### اختبار R2 Storage
```bash
./venv/bin/python manage.py shell
```
```python
from django.core.files.base import ContentFile
from django.core.files.storage import default_storage

test = ContentFile(b"Test")
path = default_storage.save('test.txt', test)
print(f"Success! File at: {path}")
default_storage.delete(path)
```

---

## 📁 هيكل الملفات

```
backend/
├── .env                    # إعدادات المشروع (لا ترفعه لـ Git!)
├── .env.supabase          # قالب: قاعدة البيانات فقط
├── .env.supabase.full     # قالب: قاعدة البيانات + R2
├── requirements.txt       # المكتبات المطلوبة
├── manage.py
└── ...

ملفات التوثيق:
├── SUPABASE_SETUP.md      # دليل Supabase
├── R2_SETUP.md            # دليل Cloudflare R2
└── FULL_SETUP_GUIDE.md    # هذا الملف
```

---

## ⚠️ حل المشاكل الشائعة

### خطأ: "No module named 'psycopg2'"
```bash
./venv/bin/pip install psycopg2-binary==2.9.9
```

### خطأ: "could not connect to server"
- تحقق من اتصالك بالإنترنت
- تحقق من صحة بيانات Supabase
- تأكد من أن IP مسموح في Supabase

### خطأ: "NoCredentialsError" أو "AccessDenied"
- تحقق من R2 API Keys
- تأكد من صلاحيات API Token
- تحقق من عدم وجود مسافات في `.env`

### الملفات لا تُحفظ في R2
- تأكد من `USE_R2_STORAGE=True`
- تحقق من صحة AWS_ACCESS_KEY_ID و AWS_SECRET_ACCESS_KEY
- أعد تشغيل السيرفر بعد تعديل `.env`

---

## 🔒 الأمان

### ✅ يجب عمله:
- احفظ `.env` محلياً فقط
- تأكد من وجود `.env` في `.gitignore`
- غيّر `SECRET_KEY` في الإنتاج
- استخدم كلمات مرور قوية

### ❌ لا تفعله:
- لا ترفع `.env` إلى Git
- لا تشارك API Keys
- لا تستخدم `DEBUG=True` في الإنتاج
- لا تشارك كلمة مرور قاعدة البيانات

---

## 💡 نصائح مهمة

1. **للتطوير**: استخدم SQLite + التخزين المحلي (أسرع)
2. **للإنتاج**: استخدم Supabase + R2 (أأمن وأفضل)
3. **Backup**: Supabase تقوم بنسخ احتياطي تلقائي
4. **Performance**: R2 يستخدم CDN عالمي من Cloudflare
5. **Cost**: كلاهما مجاني للمشاريع الصغيرة

---

## 📞 الدعم

- **Supabase Dashboard**: https://app.supabase.com
- **Cloudflare Dashboard**: https://dash.cloudflare.com
- **المشروع**: https://github.com/your-repo

---

## 🎉 الخلاصة

بعد إكمال هذه الخطوات، ستكون لديك:
- ✅ قاعدة بيانات PostgreSQL احترافية على Supabase
- ✅ تخزين ملفات سريع وآمن على Cloudflare R2
- ✅ مشروع جاهز للإنتاج
- ✅ بيانات آمنة ومحمية

**استمتع ببناء مشروعك! 🚀**

