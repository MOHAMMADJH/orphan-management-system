# تثبيت GitHub CLI على PythonAnywhere

## الطريقة الأولى: تثبيت GitHub CLI محلياً

في Bash Console على PythonAnywhere، نفذ الأوامر التالية:

```bash
# إنشاء مجلد للملفات الثنائية
mkdir -p ~/bin

# تحميل GitHub CLI
cd ~/bin
wget https://github.com/cli/cli/releases/download/v2.40.1/gh_2.40.1_linux_amd64.tar.gz

# فك الضغط
tar -xzf gh_2.40.1_linux_amd64.tar.gz

# نسخ الملف التنفيذي
cp gh_2.40.1_linux_amd64/bin/gh ~/bin/gh

# إضافة إلى PATH
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# التحقق من التثبيت
gh --version
```

## الطريقة الثانية: استخدام Git العادي (أبسط)

إذا لم تنجح الطريقة الأولى، يمكنك استخدام Git العادي:

```bash
# إنشاء مستودع جديد
git init
git add .
git commit -m "Initial commit"

# إضافة remote repository (بعد إنشائه على GitHub)
git remote add origin https://github.com/USERNAME/REPOSITORY_NAME.git
git branch -M main
git push -u origin main
```

## الطريقة الثالثة: رفع الملفات مباشرة

1. **ضغط المشروع محلياً:**
```bash
# في مجلد المشروع المحلي
tar -czf backend.tar.gz backend/
```

2. **رفع الملف المضغوط إلى PythonAnywhere:**
   - استخدم تبويب "Files" 
   - انقر على "Upload a file"
   - ارفع ملف `backend.tar.gz`

3. **فك الضغط على PythonAnywhere:**
```bash
tar -xzf backend.tar.gz
```

## الطريقة الرابعة: استخدام wget لتحميل من GitHub

```bash
# تحميل المشروع كـ ZIP
wget https://github.com/USERNAME/REPOSITORY_NAME/archive/main.zip

# فك الضغط
unzip main.zip

# نقل الملفات
mv REPOSITORY_NAME-main/backend/ ~/
```

## خطوات ما بعد التحميل:

1. **إنشاء Virtual Environment:**
```bash
python3.10 -m venv venv
source venv/bin/activate
```

2. **تثبيت المتطلبات:**
```bash
pip install -r backend/requirements_pythonanywhere.txt
```

3. **إعداد قاعدة البيانات:**
```bash
cd backend
python manage.py migrate --settings=orphan_management.settings_pythonanywhere
```

4. **إنشاء Superuser:**
```bash
python manage.py createsuperuser --settings=orphan_management.settings_pythonanywhere
```

## نصائح مهمة:

- استخدم PythonAnywhere Free Account للبداية
- تأكد من أن المسارات صحيحة
- استخدم `ls` و `pwd` للتحقق من المجلدات
- في حالة الأخطاء، تحقق من logs في تبويب "Web"
