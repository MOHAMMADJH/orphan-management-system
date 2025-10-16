# دليل شامل لاستخدام Fly.io - تقرير مفصل

## جدول المحتويات
1. [التثبيت والإعداد الأولي](#التثبيت-والإعداد-الأولي)
2. [إدارة التطبيقات](#إدارة-التطبيقات)
3. [النشر والتحديث](#النشر-والتحديث)
4. [إدارة المتغيرات البيئية](#إدارة-المتغيرات-البيئية)
5. [إدارة قواعد البيانات](#إدارة-قواعد-البيانات)
6. [مراقبة الأداء والسجلات](#مراقبة-الأداء-والسجلات)
7. [إدارة الآلات](#إدارة-الآلات)
8. [استكشاف الأخطاء](#استكشاف-الأخطاء)
9. [أوامر مفيدة إضافية](#أوامر-مفيدة-إضافية)
10. [أمثلة عملية](#أمثلة-عملية)

---

## التثبيت والإعداد الأولي

### 1. تثبيت Fly CLI

```bash
# تثبيت flyctl
curl -L https://fly.io/install.sh | sh

# إضافة flyctl إلى PATH
echo 'export FLYCTL_INSTALL="/home/$USER/.fly"' >> ~/.bashrc
echo 'export PATH="$FLYCTL_INSTALL/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# التحقق من التثبيت
flyctl version
```

### 2. تسجيل الدخول

```bash
# تسجيل الدخول إلى Fly.io
flyctl auth login

# التحقق من حالة تسجيل الدخول
flyctl auth whoami

# تسجيل الخروج
flyctl auth logout
```

---

## إدارة التطبيقات

### 1. إنشاء تطبيق جديد

```bash
# إنشاء تطبيق جديد
flyctl apps create abo-naheb-backend

# إنشاء تطبيق مع اسم عشوائي
flyctl apps create

# إنشاء تطبيق في منظمة معينة
flyctl apps create --org personal abo-naheb-backend
```

### 2. إدارة التطبيقات

```bash
# عرض جميع التطبيقات
flyctl apps list

# عرض تفاصيل تطبيق معين
flyctl apps show abo-naheb-backend

# عرض حالة التطبيق
flyctl status --app abo-naheb-backend

# حذف تطبيق
flyctl apps destroy abo-naheb-backend --yes

# تغيير اسم التطبيق
flyctl apps rename abo-naheb-backend new-app-name
```

---

## النشر والتحديث

### 1. النشر الأول

```bash
# النشر من المجلد الحالي
flyctl deploy --app abo-naheb-backend

# النشر مع تحديد مسار معين
flyctl deploy --app abo-naheb-backend --config ./custom-fly.toml

# النشر من Git repository
flyctl deploy --app abo-naheb-backend --remote-only

# النشر مع تحديد region
flyctl deploy --app abo-naheb-backend --region ams
```

### 2. تحديث التطبيق

```bash
# تحديث من آخر commit
flyctl deploy --app abo-naheb-backend

# تحديث من commit محدد
flyctl deploy --app abo-naheb-backend --image-label v1.2.3

# تحديث مع إعادة تشغيل فورية
flyctl deploy --app abo-naheb-backend --strategy immediate
```

### 3. إدارة النشرات

```bash
# عرض تاريخ النشرات
flyctl releases --app abo-naheb-backend

# عرض تفاصيل نشر محدد
flyctl releases show <release-id> --app abo-naheb-backend

# الرجوع لنشر سابق
flyctl releases rollback <release-id> --app abo-naheb-backend
```

---

## إدارة المتغيرات البيئية

### 1. إعداد المتغيرات البيئية (Secrets)

```bash
# إضافة متغير واحد
flyctl secrets set SECRET_KEY="your-secret-key" --app abo-naheb-backend

# إضافة عدة متغيرات مرة واحدة
flyctl secrets set \
  SECRET_KEY="your-secret-key" \
  DEBUG=False \
  ALLOWED_HOSTS="*.fly.dev" \
  --app abo-naheb-backend

# إضافة من ملف .env
flyctl secrets import --app abo-naheb-backend < .env

# عرض جميع المتغيرات (بدون القيم)
flyctl secrets list --app abo-naheb-backend

# حذف متغير
flyctl secrets unset SECRET_KEY --app abo-naheb-backend

# حذف جميع المتغيرات
flyctl secrets unset --app abo-naheb-backend --yes
```

### 2. متغيرات البيئة العادية

```bash
# إضافة متغير بيئة عادي (غير سري)
flyctl env set NODE_ENV=production --app abo-naheb-backend

# عرض متغيرات البيئة
flyctl env list --app abo-naheb-backend

# حذف متغير بيئة
flyctl env unset NODE_ENV --app abo-naheb-backend
```

---

## إدارة قواعد البيانات

### 1. Managed PostgreSQL

```bash
# إنشاء قاعدة بيانات PostgreSQL
flyctl mpg create --name abo-naheb-db --region ams

# عرض قواعد البيانات
flyctl mpg list

# عرض تفاصيل قاعدة بيانات
flyctl mpg show abo-naheb-db

# ربط قاعدة بيانات بتطبيق
flyctl mpg attach abo-naheb-db --app abo-naheb-backend

# فصل قاعدة بيانات عن تطبيق
flyctl mpg detach abo-naheb-db --app abo-naheb-backend

# الاتصال بقاعدة البيانات
flyctl mpg connect --cluster abo-naheb-db

# حذف قاعدة بيانات
flyctl mpg destroy abo-naheb-db --yes
```

### 2. Unmanaged PostgreSQL

```bash
# إنشاء قاعدة بيانات غير مديرة
flyctl postgres create --name abo-naheb-db --region ams --initial-cluster-size 1

# ربط قاعدة البيانات
flyctl postgres attach abo-naheb-db --app abo-naheb-backend

# الاتصال بقاعدة البيانات
flyctl postgres connect abo-naheb-db
```

---

## مراقبة الأداء والسجلات

### 1. عرض السجلات (Logs)

```bash
# عرض السجلات الحية
flyctl logs --app abo-naheb-backend

# عرض آخر 100 سطر
flyctl logs --app abo-naheb-backend -n 100

# عرض السجلات مع فلترة
flyctl logs --app abo-naheb-backend --filter error

# عرض سجلات آلة معينة
flyctl logs --app abo-naheb-backend --machine-id d8d93d2f601e18

# عرض سجلات فترة زمنية
flyctl logs --app abo-naheb-backend --since 1h
```

### 2. مراقبة الأداء

```bash
# عرض إحصائيات التطبيق
flyctl metrics --app abo-naheb-backend

# عرض استخدام الذاكرة
flyctl metrics --app abo-naheb-backend --type memory

# عرض استخدام CPU
flyctl metrics --app abo-naheb-backend --type cpu

# عرض إحصائيات HTTP
flyctl metrics --app abo-naheb-backend --type http
```

### 3. فتح التطبيق

```bash
# فتح التطبيق في المتصفح
flyctl open --app abo-naheb-backend

# فتح مسار معين
flyctl open --app abo-naheb-backend /admin/

# فتح مع معاملات
flyctl open --app abo-naheb-backend "?debug=1"
```

---

## إدارة الآلات

### 1. عرض وإدارة الآلات

```bash
# عرض جميع الآلات
flyctl machines list --app abo-naheb-backend

# عرض تفاصيل آلة معينة
flyctl machines show d8d93d2f601e18 --app abo-naheb-backend

# تشغيل آلة
flyctl machines start d8d93d2f601e18 --app abo-naheb-backend

# إيقاف آلة
flyctl machines stop d8d93d2f601e18 --app abo-naheb-backend

# إعادة تشغيل آلة
flyctl machines restart d8d93d2f601e18 --app abo-naheb-backend

# حذف آلة
flyctl machines destroy d8d93d2f601e18 --app abo-naheb-backend --force
```

### 2. SSH والوصول المباشر

```bash
# فتح SSH console
flyctl ssh console --app abo-naheb-backend

# تنفيذ أمر معين عبر SSH
flyctl ssh console --app abo-naheb-backend -C "python manage.py migrate"

# فتح SSH لآلة معينة
flyctl ssh console --app abo-naheb-backend --machine-id d8d93d2f601e18
```

---

## استكشاف الأخطاء

### 1. تشخيص المشاكل

```bash
# عرض حالة التطبيق
flyctl status --app abo-naheb-backend

# عرض تفاصيل الآلات
flyctl machines list --app abo-naheb-backend

# عرض السجلات للأخطاء
flyctl logs --app abo-naheb-backend --filter error

# عرض health checks
flyctl checks list --app abo-naheb-backend
```

### 2. إعادة التشغيل والاستعادة

```bash
# إعادة تشغيل جميع الآلات
flyctl apps restart abo-naheb-backend

# إعادة تشغيل آلة معينة
flyctl machines restart d8d93d2f601e18 --app abo-naheb-backend

# تغيير حجم الآلة
flyctl scale memory 512 --app abo-naheb-backend

# تغيير عدد الآلات
flyctl scale count 2 --app abo-naheb-backend
```

---

## أوامر مفيدة إضافية

### 1. إدارة المجلدات (Volumes)

```bash
# إنشاء مجلد
flyctl volumes create data --size 10 --region ams --app abo-naheb-backend

# عرض المجلدات
flyctl volumes list --app abo-naheb-backend

# توسيع مجلد
flyctl volumes extend data --size 20 --app abo-naheb-backend

# حذف مجلد
flyctl volumes destroy data --app abo-naheb-backend
```

### 2. إدارة الشبكات

```bash
# عرض عناوين IP
flyctl ips list --app abo-naheb-backend

# إضافة عنوان IPv4
flyctl ips allocate-v4 --app abo-naheb-backend

# إضافة عنوان IPv6
flyctl ips allocate-v6 --app abo-naheb-backend

# حذف عنوان IP
flyctl ips release <ip-address> --app abo-naheb-backend
```

### 3. إدارة الشهادات

```bash
# عرض الشهادات
flyctl certs list --app abo-naheb-backend

# إضافة شهادة
flyctl certs add abo-naheb-backend.com --app abo-naheb-backend

# حذف شهادة
flyctl certs remove abo-naheb-backend.com --app abo-naheb-backend
```

---

## أمثلة عملية

### مثال 1: نشر تطبيق Django جديد

```bash
# 1. إنشاء التطبيق
flyctl apps create my-django-app

# 2. إنشاء قاعدة بيانات
flyctl mpg create --name my-django-db --region ams

# 3. ربط قاعدة البيانات
flyctl mpg attach my-django-db --app my-django-app

# 4. إعداد المتغيرات البيئية
flyctl secrets set \
  SECRET_KEY="$(python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())')" \
  DEBUG=False \
  ALLOWED_HOSTS="*.fly.dev" \
  --app my-django-app

# 5. النشر
flyctl deploy --app my-django-app

# 6. تشغيل migrations
flyctl ssh console --app my-django-app -C "python manage.py migrate"

# 7. إنشاء superuser
flyctl ssh console --app my-django-app -C "python manage.py createsuperuser"
```

### مثال 2: مراقبة وتحديث تطبيق

```bash
# 1. فحص حالة التطبيق
flyctl status --app my-django-app

# 2. عرض السجلات
flyctl logs --app my-django-app -n 50

# 3. فحص الأداء
flyctl metrics --app my-django-app

# 4. تحديث التطبيق
git add .
git commit -m "Update application"
git push origin main
flyctl deploy --app my-django-app

# 5. فحص النتيجة
flyctl open --app my-django-app
```

### مثال 3: استكشاف مشكلة

```bash
# 1. فحص الحالة العامة
flyctl status --app my-django-app

# 2. عرض تفاصيل الآلات
flyctl machines list --app my-django-app

# 3. فحص السجلات للأخطاء
flyctl logs --app my-django-app --filter error

# 4. فحص health checks
flyctl checks list --app my-django-app

# 5. الاتصال بالآلة للفحص المباشر
flyctl ssh console --app my-django-app

# 6. إعادة تشغيل إذا لزم الأمر
flyctl apps restart my-django-app
```

### مثال 4: إدارة متقدمة

```bash
# 1. توسيع التطبيق
flyctl scale count 3 --app my-django-app
flyctl scale memory 1024 --app my-django-app

# 2. إضافة عنوان IP مخصص
flyctl ips allocate-v4 --app my-django-app

# 3. إضافة شهادة SSL
flyctl certs add myapp.com --app my-django-app

# 4. إنشاء backup لقاعدة البيانات
flyctl mpg connect --cluster my-django-db -c "pg_dump myapp > backup.sql"

# 5. مراقبة مستمرة
watch "flyctl status --app my-django-app"
```

---

## نصائح مهمة

### 1. أفضل الممارسات

```bash
# استخدم أسماء تطبيقات واضحة ومميزة
flyctl apps create my-company-api-prod

# استخدم regions قريبة من المستخدمين
flyctl deploy --region ams  # لأوروبا
flyctl deploy --region sin  # لآسيا

# راقب استخدام الموارد بانتظام
flyctl metrics --app my-app --type memory
flyctl metrics --app my-app --type cpu
```

### 2. الأمان

```bash
# استخدم secrets للمعلومات الحساسة
flyctl secrets set API_KEY="secret-key" --app my-app

# لا تضع secrets في ملفات Git
echo "*.env" >> .gitignore
echo "secrets/" >> .gitignore

# استخدم HTTPS دائماً
flyctl certs add myapp.com --app my-app
```

### 3. الأداء

```bash
# استخدم auto-scaling
flyctl scale count 1-5 --app my-app

# راقب استخدام الذاكرة
flyctl scale memory 512 --app my-app

# استخدم CDN للملفات الثابتة
# استخدم Redis للـ caching
```

---

## أوامر الطوارئ

### 1. إيقاف طارئ

```bash
# إيقاف جميع الآلات
flyctl scale count 0 --app my-app

# حذف تطبيق كامل
flyctl apps destroy my-app --yes
```

### 2. استعادة سريعة

```bash
# إعادة تشغيل جميع الآلات
flyctl apps restart my-app

# الرجوع لنشر سابق
flyctl releases rollback <release-id> --app my-app

# إعادة تشغيل آلة معينة
flyctl machines restart <machine-id> --app my-app
```

---

## خلاصة

هذا الدليل يغطي جميع الجوانب الأساسية لاستخدام Fly.io. تذكر:

1. **راقب التطبيق بانتظام** باستخدام `flyctl logs` و `flyctl metrics`
2. **استخدم secrets للمعلومات الحساسة** وليس متغيرات البيئة العادية
3. **اختبر التطبيق محلياً** قبل النشر
4. **احتفظ بنسخ احتياطية** لقواعد البيانات
5. **راقب التكاليف** باستخدام dashboard

للحصول على مساعدة إضافية:
- `flyctl help` - عرض المساعدة العامة
- `flyctl help <command>` - عرض مساعدة لأمر معين
- [Fly.io Documentation](https://fly.io/docs/) - الوثائق الرسمية

---

*تم إنشاء هذا الدليل بتاريخ: 2025-10-16*
*آخر تحديث: 2025-10-16*
