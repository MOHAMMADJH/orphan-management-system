# دليل رفع Backend على PythonAnywhere

## الخطوات المطلوبة لرفع المشروع:

### 1. رفع الملفات
1. انتقل إلى تبويب "Files" في PythonAnywhere
2. انقر على "Browse files"
3. ارفع مجلد backend كاملاً إلى المسار: `/home/mohjas/`

### 2. إنشاء Virtual Environment
1. انتقل إلى تبويب "Consoles"
2. افتح Bash Console جديد
3. نفذ الأوامر التالية:

```bash
cd /home/mohjas
python3.10 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r backend/requirements_pythonanywhere.txt
```

### 3. إعداد قاعدة البيانات
```bash
cd backend
python manage.py migrate --settings=orphan_management.settings_pythonanywhere
python manage.py collectstatic --noinput --settings=orphan_management.settings_pythonanywhere
```

### 4. إنشاء Superuser
```bash
python manage.py createsuperuser --settings=orphan_management.settings_pythonanywhere
```

### 5. إعداد Web App
1. انتقل إلى تبويب "Web"
2. انقر على "Add a new web app"
3. اختر "Manual Configuration"
4. اختر "Python 3.10"

### 6. تكوين WSGI
1. في تبويب "Web"، انقر على "WSGI configuration file"
2. استبدل محتوى الملف بالمحتوى التالي:

```python
import os
import sys

path = '/home/mohjas'
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'orphan_management.settings_pythonanywhere'

from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

### 7. إعداد Static Files
في تبويب "Web"، في قسم "Static files":
- URL: `/static/`
- Directory: `/home/mohjas/static`

### 8. إعداد Media Files
في تبويب "Web"، في قسم "Static files":
- URL: `/media/`
- Directory: `/home/mohjas/media`

### 9. تشغيل المشروع
1. انقر على "Reload" في تبويب "Web"
2. انتقل إلى رابط الموقع: `https://mohjas.pythonanywhere.com`

## ملاحظات مهمة:

- تأكد من أن جميع المتغيرات البيئية محددة بشكل صحيح
- في حالة وجود أخطاء، تحقق من logs في تبويب "Web"
- يمكنك استخدام "Tasks" لتشغيل مهام مجدولة
- تذكر تحديث CORS_ALLOWED_ORIGINS في الإعدادات

## استكشاف الأخطاء:

1. **خطأ في الاستيراد**: تأكد من أن المسارات صحيحة
2. **خطأ في قاعدة البيانات**: تأكد من تشغيل migrations
3. **خطأ في Static files**: تأكد من تشغيل collectstatic
4. **خطأ في CORS**: تأكد من إعدادات CORS في الإعدادات
