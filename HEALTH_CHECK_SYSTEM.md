# نظام فحص صحة Backend - Health Check System

## نظرة عامة

تم تطوير نظام احترافي لفحص صحة الـ backend يضمن عمل النظام بشكل صحيح قبل عرض المحتوى للمستخدمين. يوفر النظام تجربة مستخدم محسنة مع شاشات تحميل احترافية وتنبيهات فورية عند وجود مشاكل.

## الميزات الرئيسية

### 🔍 فحص شامل للصحة
- فحص الاتصال بقاعدة البيانات
- اختبار العمليات الأساسية
- فحص خدمات المصادقة
- معلومات مفصلة عن حالة النظام

### 🎨 واجهة مستخدم احترافية
- شاشة تحميل تفاعلية مع رسائل متحركة
- مؤشرات حالة بصرية (أخضر/أحمر/أزرق)
- تنبيهات عند وجود مشاكل
- تصميم متجاوب للجوال والكمبيوتر

### ⚡ أداء محسن
- فحص سريع للصفحة الأولى (3 ثوان)
- فحص شامل مع إعادة المحاولة (10 ثوان)
- فحص دوري كل 30 ثانية
- تخزين مؤقت للحالة

### 🔄 مراقبة مستمرة
- فحص تلقائي عند العودة للتبويب
- فحص عند الاتصال/انقطاع الإنترنت
- إعادة محاولة ذكية مع تأخير متزايد

## الملفات المضافة

### Backend
- **`backend/api/views.py`**: تم تحسين endpoint `/api/health/` لفحص شامل

### Frontend
- **`frontend/lib/healthService.ts`**: خدمة فحص الصحة الرئيسية
- **`frontend/app/components/HealthLoadingScreen.tsx`**: شاشة التحميل الاحترافية
- **`frontend/app/contexts/HealthContext.tsx`**: Context لإدارة حالة النظام
- **`frontend/app/layout.tsx`**: دمج HealthProvider في التطبيق الرئيسي
- **`frontend/app/(dashboard)/layout.tsx`**: إضافة مؤشرات الحالة للوحة التحكم
- **`frontend/app/page.tsx`**: إضافة مؤشر الحالة للصفحة الرئيسية

## كيفية العمل

### 1. عند فتح التطبيق
```typescript
// يتم عرض شاشة التحميل
<HealthLoadingScreen onHealthCheckComplete={handleComplete} />

// فحص سريع للصحة
const healthStatus = await healthService.quickCheck();
```

### 2. أثناء الاستخدام
```typescript
// فحص دوري كل 30 ثانية
useEffect(() => {
  const interval = setInterval(() => {
    checkHealth();
  }, 30000);
  
  return () => clearInterval(interval);
}, []);
```

### 3. عند وجود مشاكل
```typescript
// عرض تنبيه أحمر
{hasError && (
  <div className="bg-red-50 border-b border-red-200">
    <p>تحذير: مشكلة في الاتصال بالخادم</p>
    <button onClick={checkHealth}>إعادة المحاولة</button>
  </div>
)}
```

## API Response

### عند نجاح الفحص
```json
{
  "status": "healthy",
  "timestamp": "2025-01-15T10:30:00Z",
  "database": "connected",
  "services": {
    "database": true,
    "api": true,
    "authentication": true
  },
  "version": "1.0.0",
  "environment": "production"
}
```

### عند فشل الفحص
```json
{
  "status": "unhealthy",
  "timestamp": "2025-01-15T10:30:00Z",
  "error": "Database connection failed",
  "services": {
    "database": false,
    "api": true,
    "authentication": false
  }
}
```

## التخصيص

### تغيير فترات الفحص
```typescript
<HealthProvider 
  enablePeriodicChecks={true} 
  checkInterval={60000} // 60 ثانية
  showLoadingScreen={true}
>
```

### تخصيص شاشة التحميل
```typescript
<HealthLoadingScreen 
  onHealthCheckComplete={handleComplete}
  showDetailedStatus={true}
/>
```

### استخدام مؤشر الحالة
```typescript
import { HealthStatusIndicator } from '@/app/contexts/HealthContext';

// مؤشر بسيط
<HealthStatusIndicator showText={false} />

// مؤشر مع نص
<HealthStatusIndicator showText={true} />
```

## الاستخدام في المكونات

### فحص الحالة يدوياً
```typescript
import { useHealth } from '@/app/contexts/HealthContext';

function MyComponent() {
  const { healthStatus, isChecking, checkHealth } = useHealth();
  
  if (healthStatus?.status === 'unhealthy') {
    return <div>النظام غير متاح حالياً</div>;
  }
  
  return <div>المحتوى العادي</div>;
}
```

### الاستجابة لتغيير الحالة
```typescript
useEffect(() => {
  if (healthStatus?.status === 'unhealthy') {
    // إظهار تنبيه أو تعطيل ميزات معينة
    showNotification('مشكلة في الاتصال بالخادم');
  }
}, [healthStatus]);
```

## الأمان والأداء

### الأمان
- فحص الصحة لا يتطلب مصادقة (AllowAny)
- لا يتم كشف معلومات حساسة في الاستجابة
- حماية من هجمات DoS مع timeout

### الأداء
- فحص سريع للصفحة الأولى (3 ثوان)
- تخزين مؤقت للحالة لتجنب الفحوصات المتكررة
- فحص دوري فقط عند الحاجة

## استكشاف الأخطاء

### مشاكل شائعة

1. **شاشة التحميل لا تختفي**
   - تحقق من اتصال الإنترنت
   - تحقق من أن الـ backend يعمل
   - تحقق من URL في `NEXT_PUBLIC_API_URL`

2. **مؤشر الحالة يظهر دائماً أحمر**
   - تحقق من endpoint `/api/health/`
   - تحقق من قاعدة البيانات
   - تحقق من إعدادات CORS

3. **فحص دوري لا يعمل**
   - تأكد من تفعيل `enablePeriodicChecks`
   - تحقق من قيمة `checkInterval`

### سجلات التطوير
```typescript
// تفعيل السجلات في وضع التطوير
console.log('Health Status:', healthStatus);
console.log('Is Checking:', isChecking);
console.log('Has Error:', hasError);
```

## التحديثات المستقبلية

### ميزات مخططة
- [ ] إشعارات push عند وجود مشاكل
- [ ] إحصائيات مفصلة عن أوقات الاستجابة
- [ ] دعم لفحص خدمات خارجية
- [ ] لوحة تحكم للمراقبة

### تحسينات الأداء
- [ ] فحص متوازي لخدمات متعددة
- [ ] تحسين خوارزمية إعادة المحاولة
- [ ] ضغط استجابات API

---

## الدعم

للحصول على المساعدة أو الإبلاغ عن مشاكل، يرجى التواصل مع فريق التطوير.

**تم التطوير بـ ❤️ لضمان أفضل تجربة مستخدم**
