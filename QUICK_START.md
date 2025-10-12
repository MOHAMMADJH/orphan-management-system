# 🚀 Quick Start Guide

دليل سريع للبدء في التطوير المحلي

## للبدء السريع (5 دقائق) ⚡

### 1. Backend Setup

```bash
cd backend
./setup-dev.sh
source venv/bin/activate
python manage.py createsuperuser
python manage.py runserver
```

✅ Backend جاهز على: http://localhost:8000

### 2. Frontend Setup

```bash
cd frontend
npm install
cp env.local.example .env.local
npm run dev
```

✅ Frontend جاهز على: http://localhost:3000

### 3. Login

افتح المتصفح على: http://localhost:3000/login

استخدم بيانات الـ superuser التي أنشأتها

## ما تم إعداده؟

✅ **Backend (Django)**
- قاعدة بيانات SQLite (db.sqlite3)
- ملفات محلية في مجلد media/
- API جاهز للاستخدام
- Admin panel على /admin

✅ **Frontend (Next.js)**
- TypeScript + Tailwind CSS
- API client جاهز
- Authentication system
- PWA support

## الخطوات التالية

1. **اختبر API**: http://localhost:8000/api/
2. **افتح Admin Panel**: http://localhost:8000/admin/
3. **ابدأ التطوير**: Frontend على http://localhost:3000/

## للتبديل للإنتاج لاحقاً

عندما تكون جاهزاً للنشر:

1. انسخ ملف الإنتاج:
```bash
cd backend
cp env.production.example .env
```

2. حدّث الإعدادات:
```bash
USE_POSTGRES=True
USE_R2_STORAGE=True
```

3. أضف بيانات PostgreSQL و R2

4. ثبت المكتبات الإضافية:
```bash
pip install -r requirements-production.txt
```

## المساعدة

- للمزيد من التفاصيل: انظر `README.md`
- لمشاكل Backend: انظر `backend/README.md`
- لمشاكل Frontend: انظر `frontend/README.md`

## مشاكل شائعة

### Backend لا يعمل؟
```bash
cd backend
source venv/bin/activate
python manage.py migrate
python manage.py runserver
```

### Frontend لا يعمل؟
```bash
cd frontend
rm -rf node_modules .next
npm install
npm run dev
```

### نسيت كلمة مرور Admin؟
```bash
cd backend
source venv/bin/activate
python manage.py changepassword <username>
```

