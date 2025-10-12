# دليل رفع Backend و Frontend على GitHub

تم فصل المشروع إلى repositories منفصلة. اتبع الخطوات التالية لرفع كل واحد على GitHub.

## ✅ تم إنجازه

- [x] إنشاء `.gitignore` مخصص للـ Backend
- [x] إنشاء `.gitignore` مخصص للـ Frontend
- [x] إنشاء `README.md` للـ Backend
- [x] إنشاء `README.md` للـ Frontend
- [x] تهيئة Git repository في مجلد backend
- [x] تهيئة Git repository في مجلد frontend
- [x] إنشاء commit أولي لكل repository

## 📋 خطوات رفع Backend على GitHub

### 1. إنشاء Repository جديد على GitHub للـ Backend

1. اذهب إلى [GitHub](https://github.com) وسجل دخول
2. اضغط على زر "New Repository" أو "+"
3. أدخل اسم Repository (مثال: `orphan-management-backend`)
4. اختر Public أو Private حسب رغبتك
5. **لا تختر** "Initialize with README" (لأنه موجود بالفعل)
6. اضغط "Create Repository"

### 2. ربط Backend المحلي مع GitHub

```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend

# استبدل USERNAME باسم المستخدم الخاص بك
# استبدل REPO_NAME باسم الـ repository الذي أنشأته
git remote add origin https://github.com/USERNAME/REPO_NAME.git

# إذا كنت تفضل استخدام main بدلاً من master
git branch -M main

# رفع الملفات إلى GitHub
git push -u origin main
```

مثال:
```bash
git remote add origin https://github.com/mohjas/orphan-management-backend.git
git branch -M main
git push -u origin main
```

---

## 📋 خطوات رفع Frontend على GitHub

### 1. إنشاء Repository جديد على GitHub للـ Frontend

1. اذهب إلى [GitHub](https://github.com) وسجل دخول
2. اضغط على زر "New Repository" أو "+"
3. أدخل اسم Repository (مثال: `orphan-management-frontend`)
4. اختر Public أو Private حسب رغبتك
5. **لا تختر** "Initialize with README" (لأنه موجود بالفعل)
6. اضغط "Create Repository"

### 2. ربط Frontend المحلي مع GitHub

```bash
cd /home/mohjas/CascadeProjects/AboNaheb/frontend

# استبدل USERNAME باسم المستخدم الخاص بك
# استبدل REPO_NAME باسم الـ repository الذي أنشأته
git remote add origin https://github.com/USERNAME/REPO_NAME.git

# إذا كنت تفضل استخدام main بدلاً من master
git branch -M main

# رفع الملفات إلى GitHub
git push -u origin main
```

مثال:
```bash
git remote add origin https://github.com/mohjas/orphan-management-frontend.git
git branch -M main
git push -u origin main
```

---

## 🔐 التعامل مع المصادقة

إذا طلب منك GitHub اسم المستخدم وكلمة المرور:

### خيار 1: استخدام Personal Access Token (مُستحسن)

1. اذهب إلى GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. اضغط "Generate new token (classic)"
3. أعط التوكن اسم (مثل "Git CLI")
4. اختر صلاحية `repo` (full control of private repositories)
5. احفظ التوكن في مكان آمن
6. استخدمه كـ password عند الـ push

### خيار 2: استخدام SSH

```bash
# إنشاء SSH key إذا لم يكن موجود
ssh-keygen -t ed25519 -C "your_email@example.com"

# نسخ المفتاح العام
cat ~/.ssh/id_ed25519.pub

# أضف المفتاح إلى GitHub:
# Settings → SSH and GPG keys → New SSH key
```

ثم استخدم SSH URL بدلاً من HTTPS:
```bash
# للـ Backend
git remote set-url origin git@github.com:USERNAME/orphan-management-backend.git

# للـ Frontend
git remote set-url origin git@github.com:USERNAME/orphan-management-frontend.git
```

---

## 📝 التعديلات المستقبلية

### للـ Backend:
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/backend
git add .
git commit -m "وصف التعديل"
git push
```

### للـ Frontend:
```bash
cd /home/mohjas/CascadeProjects/AboNaheb/frontend
git add .
git commit -m "وصف التعديل"
git push
```

---

## 📂 البنية النهائية

```
AboNaheb/
├── backend/           → Repository منفصل (orphan-management-backend)
│   ├── .git/
│   ├── .gitignore
│   ├── README.md
│   └── ...
│
├── frontend/          → Repository منفصل (orphan-management-frontend)
│   ├── .git/
│   ├── .gitignore
│   ├── README.md
│   └── ...
│
└── GIT_SETUP_GUIDE.md  → هذا الملف (للمرجع فقط)
```

---

## ✨ نصائح إضافية

1. **التأكد من Remote**:
   ```bash
   git remote -v
   ```

2. **حذف Remote خاطئ**:
   ```bash
   git remote remove origin
   ```

3. **تحديث من GitHub**:
   ```bash
   git pull origin main
   ```

4. **عرض Commits**:
   ```bash
   git log --oneline
   ```

---

## 🎉 بعد الرفع

- شارك روابط الـ repositories:
  - Backend: `https://github.com/USERNAME/orphan-management-backend`
  - Frontend: `https://github.com/USERNAME/orphan-management-frontend`

- يمكنك إضافة badges في README:
  ```markdown
  ![Python](https://img.shields.io/badge/python-3.11+-blue.svg)
  ![Django](https://img.shields.io/badge/django-5.0-green.svg)
  ```

- يمكنك إعداد GitHub Actions للـ CI/CD

---

**ملاحظة**: تأكد من عدم رفع ملفات `.env` أو بيانات حساسة. الـ `.gitignore` المُعد يمنع ذلك تلقائياً.

