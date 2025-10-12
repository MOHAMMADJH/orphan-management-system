# أيتام شرق الزيتون | East Zeitoun Orphans

<div align="center">

![Version](https://img.shields.io/badge/version-0.1.0-blue.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

**نظام متكامل لإدارة ورعاية أيتام شرق الزيتون**

[Demo](https://your-demo-url.vercel.app) • [Documentation](./VERCEL_DEPLOYMENT.md) • [Report Bug](https://github.com/yourusername/repo/issues)

</div>

---

## 📋 نظرة عامة

نظام شامل لإدارة ورعاية الأيتام، يوفر:
- 🏠 **صفحة Landing Page احترافية** موجهة للجمهور والمتبرعين
- 📊 **نظام إدارة متكامل** لتتبع بيانات الأيتام
- 📝 **إدارة الاستمارات** لتسجيل الحالات الجديدة
- 💰 **عرض الحملات** والمساعدات المقدمة
- 📈 **تقارير وإحصائيات** تفصيلية
- 👥 **إدارة المستخدمين** مع صلاحيات متعددة

---

## 🌟 المميزات الرئيسية

### 🎨 Frontend (Next.js 15.5.4)

#### صفحة Landing Page:
- ✨ Hero section جذاب مع تأثيرات متحركة
- 📊 عرض إحصائيات حية (عدد الأيتام، المتبرعين، الحملات)
- 🎯 3 حملات نشطة مع progress bars
- 🤲 6 أنواع من المساعدات المقدمة
- 💬 قصص نجاح ملهمة
- 💚 Call-to-action للتبرع
- 🛡️ قسم الشفافية والمصداقية
- 📱 تصميم responsive كامل

#### نظام الإدارة:
- 🔐 نظام مصادقة JWT
- 📋 Dashboard تفاعلي
- 📝 إدارة الاستمارات (CRUD)
- 👶 إدارة ملفات الأيتام
- 📊 إحصائيات تفاعلية مع Charts
- 👥 إدارة المستخدمين
- 🖼️ رفع وإدارة الصور والمرفقات
- 📱 PWA Support

### 🔧 Backend (Django 5.0 + DRF)

- 🔒 JWT Authentication
- 📊 RESTful API
- 📁 دعم Cloudflare R2 / AWS S3
- 🗄️ PostgreSQL / SQLite
- 📋 نظام الاستمارات المتقدم
- 👥 إدارة المستخدمين والصلاحيات
- 📈 Aggregated Statistics
- 🖼️ معالجة الصور والملفات

---

## 🛠️ التقنيات المستخدمة

### Frontend:
- **Framework:** Next.js 15.5.4 (App Router)
- **Language:** TypeScript 5
- **Styling:** Tailwind CSS 4
- **Forms:** React Hook Form + Zod
- **Charts:** Chart.js + react-chartjs-2
- **HTTP Client:** Axios
- **PWA:** next-pwa
- **QR Codes:** qrcode.react

### Backend:
- **Framework:** Django 5.0
- **API:** Django REST Framework
- **Auth:** JWT (djangorestframework-simplejwt)
- **Database:** PostgreSQL / SQLite
- **Storage:** Cloudflare R2 / AWS S3
- **CORS:** django-cors-headers
- **Reports:** ReportLab, OpenPyXL

---

## 📦 التثبيت والتشغيل

### المتطلبات الأساسية:
- Node.js 20+
- Python 3.11+
- PostgreSQL (اختياري، SQLite متوفر للتطوير)

### Frontend Setup:

```bash
# انتقل إلى مجلد Frontend
cd frontend

# تثبيت المكتبات
npm install

# نسخ ملف البيئة
cp env.local.example .env.local

# تعديل المتغيرات البيئية
# NEXT_PUBLIC_API_URL=http://localhost:8000/api

# تشغيل في وضع التطوير
npm run dev

# البناء للإنتاج
npm run build
npm start
```

الموقع سيعمل على: http://localhost:3000

### Backend Setup:

```bash
# انتقل إلى مجلد Backend
cd backend

# إنشاء بيئة افتراضية
python -m venv venv
source venv/bin/activate  # على Linux/Mac
# أو
venv\Scripts\activate  # على Windows

# تثبيت المكتبات
pip install -r requirements.txt

# تشغيل Migrations
python manage.py migrate

# إنشاء مستخدم admin
python manage.py createsuperuser

# تشغيل الخادم
python manage.py runserver
```

الـ API سيعمل على: http://localhost:8000

---

## 🚀 النشر على Vercel

### الخطوة 1: نشر Backend
أولاً، انشر Backend على خدمة مثل Render أو Railway:
```bash
cd backend
# اتبع التعليمات في DEPLOYMENT.md
```

### الخطوة 2: نشر Frontend
```bash
cd frontend
vercel --prod
```

### الخطوة 3: إضافة متغيرات البيئة
في Vercel Dashboard:
```
NEXT_PUBLIC_API_URL=https://your-backend-url.com/api
```

### الخطوة 4: تحديث CORS
في `backend/orphan_management/settings.py`:
```python
CORS_ALLOWED_ORIGINS = [
    "https://your-vercel-app.vercel.app",
]
```

📖 للمزيد من التفاصيل، راجع [VERCEL_DEPLOYMENT.md](./VERCEL_DEPLOYMENT.md)

---

## 📁 هيكل المشروع

```
AboNaheb/
├── frontend/                 # Next.js Frontend
│   ├── app/                 # App Router Pages
│   │   ├── (dashboard)/     # Dashboard Pages
│   │   ├── form/           # Form Pages
│   │   ├── login/          # Login Page
│   │   └── page.tsx        # Landing Page
│   ├── lib/                # Utilities & API
│   ├── types/              # TypeScript Types
│   ├── public/             # Static Assets
│   └── package.json
│
├── backend/                 # Django Backend
│   ├── api/                # Main App
│   │   ├── models.py       # Database Models
│   │   ├── views.py        # API Views
│   │   ├── serializers.py  # DRF Serializers
│   │   └── urls.py         # API Routes
│   ├── orphan_management/  # Project Settings
│   ├── media/              # Uploaded Files
│   └── requirements.txt
│
└── README.md               # هذا الملف
```

---

## 🎯 الاستخدام

### للمتبرعين والجمهور:
1. افتح الصفحة الرئيسية
2. تصفح الحملات النشطة
3. اقرأ قصص النجاح
4. انقر على "تبرع الآن"

### للإدارة:
1. انقر على "الإدارة" في الـ header
2. سجل دخولك باسم المستخدم وكلمة المرور
3. استخدم Dashboard لإدارة:
   - الاستمارات
   - ملفات الأيتام
   - المستخدمين
   - الإحصائيات

---

## 📸 لقطات الشاشة

### Landing Page
![Landing Page](./screenshots/landing.png)

### Dashboard
![Dashboard](./screenshots/dashboard.png)

### Orphans Management
![Orphans](./screenshots/orphans.png)

---

## 🔒 الأمان

- ✅ JWT Authentication
- ✅ Refresh Token Rotation
- ✅ CORS Protection
- ✅ HTTPS Ready
- ✅ Environment Variables
- ✅ Role-based Access Control
- ✅ Secure File Upload

---

## 🧪 الاختبار

### Frontend:
```bash
cd frontend
npm run build  # يجب أن ينجح بدون أخطاء
```

### Backend:
```bash
cd backend
python manage.py test
```

---

## 📝 المتغيرات البيئية

### Frontend (.env.local):
```env
NEXT_PUBLIC_API_URL=http://localhost:8000/api
```

### Backend (settings.py or .env):
```env
SECRET_KEY=your-secret-key
DEBUG=False
ALLOWED_HOSTS=your-domain.com
DATABASE_URL=postgresql://...
USE_R2_STORAGE=True
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
```

---

## 🤝 المساهمة

نرحب بالمساهمات! لكل مساهمة:

1. Fork المشروع
2. أنشئ branch جديد (`git checkout -b feature/amazing`)
3. Commit التغييرات (`git commit -m 'Add amazing feature'`)
4. Push إلى Branch (`git push origin feature/amazing`)
5. افتح Pull Request

---

## 📄 الترخيص

هذا المشروع مرخص تحت MIT License - انظر ملف [LICENSE](LICENSE) للتفاصيل.

---

## 👥 الفريق

- **المطور:** Your Name
- **التصميم:** Your Name
- **الإشراف:** Organization Name

---

## 📞 التواصل

- **البريد الإلكتروني:** info@eastzeitoun.org
- **الهاتف:** +970 123 456 789
- **الموقع:** https://eastzeitoun.org
- **GitHub:** https://github.com/yourusername/east-zeitoun-orphans

---

## 🙏 شكر وتقدير

شكراً لكل من ساهم في هذا المشروع الإنساني.

---

<div align="center">

**صنع بـ ❤️ لأيتام شرق الزيتون**

[⬆ العودة للأعلى](#أيتام-شرق-الزيتون--east-zeitoun-orphans)

</div>
