# 🚀 دليل النشر (Deployment Guide)

## البيئات المتاحة

### 🔧 التطوير المحلي (Development)
```bash
USE_POSTGRES=False
USE_R2_STORAGE=False
```
- ✅ SQLite (db.sqlite3)
- ✅ Local Storage (media/)
- ✅ لا يحتاج خدمات خارجية
- ✅ سريع وسهل

### 🚀 الإنتاج (Production)
```bash
USE_POSTGRES=True
USE_R2_STORAGE=True
```
- ✅ PostgreSQL
- ✅ Cloudflare R2
- ✅ Scalable & Secure
- ✅ جاهز للإنتاج

## السيناريوهات المختلفة

### 1️⃣ تطوير محلي بالكامل (موصى به للبداية)

```bash
cd backend
./setup-dev.sh
source venv/bin/activate
python manage.py createsuperuser
python manage.py runserver
```

**الإعدادات في `.env`:**
```env
SECRET_KEY=dev-secret-key
DEBUG=True
USE_POSTGRES=False
USE_R2_STORAGE=False
```

**المميزات:**
- ⚡ لا يحتاج تثبيت PostgreSQL
- ⚡ لا يحتاج حساب Cloudflare
- ⚡ جاهز للعمل فوراً

---

### 2️⃣ تطوير مع PostgreSQL فقط

إذا أردت اختبار PostgreSQL بدون R2:

```bash
# 1. Install PostgreSQL
sudo pacman -S postgresql
sudo -u postgres initdb -D /var/lib/postgres/data
sudo systemctl start postgresql
sudo systemctl enable postgresql

# 2. Create database
sudo -u postgres psql -c "CREATE DATABASE orphan_db;"
sudo -u postgres psql -c "CREATE USER postgres WITH PASSWORD 'postgres';"
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE orphan_db TO postgres;"

# 3. Update .env
USE_POSTGRES=True
USE_R2_STORAGE=False
DB_NAME=orphan_db
DB_USER=postgres
DB_PASSWORD=postgres
DB_HOST=localhost
DB_PORT=5432

# 4. Install production dependencies
pip install -r requirements-production.txt

# 5. Migrate
python manage.py migrate
```

---

### 3️⃣ تطوير مع R2 فقط

إذا أردت اختبار R2 بدون PostgreSQL:

```bash
# 1. Get R2 credentials from Cloudflare
# 2. Update .env
USE_POSTGRES=False
USE_R2_STORAGE=True
AWS_ACCESS_KEY_ID=your-key-id
AWS_SECRET_ACCESS_KEY=your-secret
AWS_STORAGE_BUCKET_NAME=your-bucket
AWS_S3_ENDPOINT_URL=https://your-account.r2.cloudflarestorage.com
AWS_S3_CUSTOM_DOMAIN=your-domain.com

# 3. Test
python manage.py runserver
```

---

### 4️⃣ إنتاج كامل (PostgreSQL + R2)

```bash
cd backend
./setup-production.sh

# Edit .env with production credentials
nano .env

# Set:
USE_POSTGRES=True
USE_R2_STORAGE=True
DEBUG=False
SECRET_KEY=generate-strong-key-here

# Add all credentials for PostgreSQL and R2
```

## Cloudflare R2 Setup

### 1. إنشاء Bucket

1. افتح Cloudflare Dashboard
2. اذهب إلى R2 Object Storage
3. انقر Create Bucket
4. أدخل اسم الـ bucket
5. اختر الموقع

### 2. إنشاء API Tokens

1. في صفحة R2، انقر Manage R2 API Tokens
2. انقر Create API Token
3. اختار Read & Write permissions
4. احفظ:
   - Access Key ID
   - Secret Access Key

### 3. تحديث .env

```env
USE_R2_STORAGE=True
AWS_ACCESS_KEY_ID=your-access-key-id
AWS_SECRET_ACCESS_KEY=your-secret-access-key
AWS_STORAGE_BUCKET_NAME=your-bucket-name
AWS_S3_ENDPOINT_URL=https://your-account-id.r2.cloudflarestorage.com
AWS_S3_CUSTOM_DOMAIN=your-custom-domain.com  # (optional)
```

### 4. اختبار الرفع

```python
python manage.py shell

from django.core.files.uploadedfile import SimpleUploadedFile
from api.models import Attachment, FormSubmission

# Test upload
file = SimpleUploadedFile("test.txt", b"Test content")
# Create attachment...
```

## PostgreSQL Setup (للإنتاج)

### على Manjaro

```bash
# Install
sudo pacman -S postgresql

# Initialize
sudo -u postgres initdb -D /var/lib/postgres/data

# Start service
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Create database
sudo -u postgres psql
CREATE DATABASE orphan_db;
CREATE USER orphan_user WITH PASSWORD 'strong-password-here';
GRANT ALL PRIVILEGES ON DATABASE orphan_db TO orphan_user;
\q
```

### Update .env

```env
USE_POSTGRES=True
DB_NAME=orphan_db
DB_USER=orphan_user
DB_PASSWORD=strong-password-here
DB_HOST=localhost
DB_PORT=5432
```

## نصائح الأمان للإنتاج

### 1. Secret Key
```bash
# Generate strong secret key
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

### 2. Debug Mode
```env
DEBUG=False
```

### 3. Allowed Hosts
في `settings.py`، حدث:
```python
ALLOWED_HOSTS = ['your-domain.com', 'www.your-domain.com']
```

### 4. CORS
حدث في `settings.py`:
```python
CORS_ALLOWED_ORIGINS = [
    "https://your-frontend-domain.com",
]
```

## الاختبار

### اختبار SQLite
```bash
python manage.py runserver
# Test upload - سيتم الحفظ في media/
```

### اختبار PostgreSQL
```bash
# Check connection
python manage.py dbshell
\dt  # List tables
\q
```

### اختبار R2
```bash
# Upload file and check Cloudflare dashboard
```

## Troubleshooting

### مشكلة: Cannot connect to PostgreSQL
```bash
sudo systemctl status postgresql
sudo systemctl start postgresql
```

### مشكلة: R2 upload fails
- تأكد من صحة credentials
- تأكد من permissions على R2 bucket
- تحقق من CORS settings في R2

### مشكلة: Migrations fail
```bash
# Reset migrations (development only!)
rm api/migrations/0*.py
python manage.py makemigrations
python manage.py migrate
```

## الخلاصة

| البيئة | SQLite | PostgreSQL | Local Storage | R2 Storage |
|--------|--------|------------|---------------|------------|
| Dev (Default) | ✅ | ❌ | ✅ | ❌ |
| Dev + PG | ❌ | ✅ | ✅ | ❌ |
| Dev + R2 | ✅ | ❌ | ❌ | ✅ |
| Production | ❌ | ✅ | ❌ | ✅ |

**التوصية:** ابدأ بـ Dev (Default) ثم انتقل تدريجياً للإنتاج

