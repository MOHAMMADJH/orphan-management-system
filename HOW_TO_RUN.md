# 🚀 كيفية تشغيل النظام

## ✅ النظام يعمل الآن!

### **Frontend (Next.js)**
- **URL**: http://localhost:3000
- **الحالة**: ✅ يعمل

### **Backend (Django)**
- **URL**: http://localhost:8000
- **Admin**: http://localhost:8000/admin/
- **API**: http://localhost:8000/api/
- **الحالة**: ✅ يعمل

---

## 🔑 بيانات الدخول
```
Username: admin
Password: admin123
```

---

## 🔧 كيفية التشغيل (إذا توقف)

### 1️⃣ تشغيل Backend
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
source venv/bin/activate
python manage.py runserver
```

### 2️⃣ تشغيل Frontend
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/frontend
npm run dev
```

---

## 🛑 إيقاف السيرفر

### إيقاف Frontend
```bash
# البحث عن العملية
ps aux | grep "next dev" | grep -v grep

# إيقاف العملية (استبدل PID برقم العملية)
kill PID

# أو استخدم pkill
pkill -f "next dev"
```

### إيقاف Backend
```bash
# البحث عن العملية
ps aux | grep "manage.py runserver" | grep -v grep

# إيقاف العملية
pkill -f "manage.py runserver"
```

---

## 🔍 التحقق من البورتات

### تحقق من البورت 3000 (Frontend)
```bash
lsof -i :3000
```

### تحقق من البورت 8000 (Backend)
```bash
lsof -i :8000
```

---

## 📱 الصفحات المتاحة

### صفحات عامة
- `/` - الصفحة الرئيسية (تحويل تلقائي)
- `/login` - تسجيل الدخول
- `/form/[id]` - الفورم العام

### لوحة التحكم (تحتاج تسجيل دخول)
- `/dashboard` - لوحة التحكم الرئيسية
- `/forms` - إدارة الروابط
- `/orphans` - بيانات الأيتام
- `/statistics` - الإحصائيات
- `/users` - إدارة المستخدمين (للمدير فقط)

---

## ⚠️ حل المشاكل الشائعة

### المشكلة: "Port already in use"
```bash
# إيقاف العملية على البورت 3000
pkill -f "next dev"

# أو حدد البورت يدوياً
cd frontend
PORT=3001 npm run dev
```

### المشكلة: "Module not found"
```bash
cd frontend
npm install
```

### المشكلة: "Database is locked"
```bash
cd backend
rm db.sqlite3
python manage.py migrate
python manage.py createsuperuser
```

### المشكلة: خطأ في CSS
```bash
cd frontend
rm -rf .next
npm run dev
```

---

## 📊 الأوامر المفيدة

### Frontend
```bash
# بناء للإنتاج
npm run build

# تشغيل الإنتاج
npm start

# فحص الأخطاء
npm run lint
```

### Backend
```bash
# تشغيل migrations
python manage.py makemigrations
python manage.py migrate

# إنشاء superuser
python manage.py createsuperuser

# جمع الملفات الثابتة
python manage.py collectstatic

# فتح shell
python manage.py shell
```

---

## 🎯 الخطوات التالية

1. افتح المتصفح على http://localhost:3000
2. سجل الدخول بـ: admin / admin123
3. جرب إنشاء رابط فورم
4. شارك الرابط واختبر الفورم
5. راجع البيانات في لوحة التحكم

---

## 📞 معلومات إضافية

- **المشروع**: نظام إدارة الأيتام والشهداء
- **Frontend**: Next.js 15 + TypeScript + Tailwind
- **Backend**: Django 5.0 + DRF
- **Database**: SQLite (تطوير) / PostgreSQL (إنتاج)
- **Storage**: محلي (تطوير) / Cloudflare R2 (إنتاج)

---

✅ **النظام جاهز للاستخدام!** 🚀
