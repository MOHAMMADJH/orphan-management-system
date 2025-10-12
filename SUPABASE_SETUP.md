# دليل ربط قاعدة بيانات Supabase

## معلومات الاتصال
- **Database Host:** db.yqxuazhduwtkcsbeqmxn.supabase.co
- **Database Name:** postgres
- **Database User:** postgres
- **Database Password:** aziton-abo-nageb-db
- **Port:** 5432
- **Connection String:** postgresql://postgres:aziton-abo-nageb-db@db.yqxuazhduwtkcsbeqmxn.supabase.co:5432/postgres

---

## خطوات الإعداد

### 1. تثبيت مكتبة PostgreSQL
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
./venv/bin/pip install psycopg2-binary==2.9.9
```

### 2. إعداد ملف البيئة
قم بنسخ الإعدادات إلى ملف `.env`:
```bash
cp .env.supabase .env
```

أو قم بإنشاء ملف `.env` يدوياً في مجلد `backend` بالمحتوى التالي:
```env
SECRET_KEY=django-insecure-sayzc-6o(6#9=2g0!vb4*92h1ks*qee&t2n*8k*(-tt)k^hrh3
DEBUG=True

USE_POSTGRES=True

DB_NAME=postgres
DB_USER=postgres
DB_PASSWORD=aziton-abo-nageb-db
DB_HOST=db.yqxuazhduwtkcsbeqmxn.supabase.co
DB_PORT=5432

USE_R2_STORAGE=False
```

### 3. تطبيق Migrations على قاعدة البيانات
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
./venv/bin/python manage.py migrate
```

### 4. إنشاء مستخدم إداري
```bash
./venv/bin/python manage.py createsuperuser
```

### 5. تشغيل الخادم
```bash
./venv/bin/python manage.py runserver
```

---

## اختبار الاتصال
يمكنك اختبار الاتصال بقاعدة البيانات باستخدام:
```bash
./venv/bin/python manage.py dbshell
```

أو تشغيل الأمر:
```bash
./venv/bin/python manage.py check --database default
```

---

## ملاحظات مهمة

### الأمان
- لا تشارك ملف `.env` مع أحد
- تأكد من أن ملف `.env` موجود في `.gitignore`
- غيّر `SECRET_KEY` في الإنتاج

### Supabase
- تأكد من أن IP الخاص بك مسموح في إعدادات Supabase
- يمكنك إدارة قاعدة البيانات من لوحة تحكم Supabase: https://app.supabase.com

### الاتصال بالإنترنت
- يتطلب المشروع اتصال بالإنترنت للوصول إلى Supabase
- في حالة عدم وجود إنترنت، استخدم SQLite المحلية:
  ```env
  USE_POSTGRES=False
  ```

---

## استكشاف الأخطاء

### خطأ: "could not connect to server"
- تحقق من اتصالك بالإنترنت
- تأكد من صحة بيانات الاتصال
- تحقق من إعدادات جدار الحماية

### خطأ: "password authentication failed"
- تأكد من صحة كلمة المرور: `aziton-abo-nageb-db`
- تحقق من إعدادات المستخدم في Supabase

### خطأ: "No module named 'psycopg2'"
- قم بتثبيت المكتبة:
  ```bash
  ./venv/bin/pip install psycopg2-binary==2.9.9
  ```

---

## أوامر سريعة

```bash
# الانتقال إلى مجلد Backend
cd /home/mohjas/CascadeProjects/AboNaheb/backend

# تفعيل البيئة الافتراضية (إذا لزم الأمر)
source venv/bin/activate

# تثبيت المكتبات
pip install -r requirements.txt

# تطبيق Migrations
python manage.py migrate

# تشغيل الخادم
python manage.py runserver
```

