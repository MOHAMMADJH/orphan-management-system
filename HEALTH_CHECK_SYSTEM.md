# نظام فحص صحة Backend المتقدم - Advanced Multi-Server Health Check System

## نظرة عامة

تم تطوير نظام احترافي ومتقدم لفحص صحة الـ backend يدعم خوادم متعددة مع آلية تبديل تلقائي. يضمن النظام عمل النظام بشكل صحيح قبل عرض المحتوى للمستخدمين مع دعم خوادم احتياطية لضمان الاستمرارية.

## الميزات الرئيسية

### 🔍 فحص شامل للصحة
- فحص الاتصال بقاعدة البيانات
- اختبار العمليات الأساسية
- فحص خدمات المصادقة
- معلومات مفصلة عن حالة النظام

### 🌐 دعم خوادم متعددة
- فحص متزامن لخوادم متعددة
- خادم محلي: `http://localhost:8000`
- خادم سحابي: `https://aziton-abo-nageb-backend.onrender.com`
- تبديل تلقائي للخادم الأفضل
- أولوية للخادم المحلي عند توافره

### 🎨 واجهة مستخدم احترافية
- شاشة تحميل تفاعلية مع رسائل متحركة
- مؤشرات حالة بصرية (أخضر/أحمر/أزرق)
- لوحة حالة الخوادم في صفحة الإحصائيات
- تنبيهات عند وجود مشاكل
- تصميم متجاوب للجوال والكمبيوتر

### ⚡ أداء محسن
- فحص سريع للصفحة الأولى (3 ثوان)
- فحص شامل مع إعادة المحاولة (10 ثوان)
- فحص دوري كل 30 ثانية
- تخزين مؤقت للحالة
- فحص متوازي للخوادم المتعددة

### 🔄 مراقبة مستمرة
- فحص تلقائي عند العودة للتبويب
- فحص عند الاتصال/انقطاع الإنترنت
- إعادة محاولة ذكية مع تأخير متزايد
- تبديل تلقائي عند فشل خادم

## الملفات المضافة

### Backend
- **`backend/api/views.py`**: تم تحسين endpoint `/api/health/` لفحص شامل

### Frontend
- **`frontend/lib/healthService.ts`**: خدمة فحص الصحة المتقدمة مع دعم خوادم متعددة
- **`frontend/app/components/HealthLoadingScreen.tsx`**: شاشة التحميل الاحترافية مع معلومات الخوادم
- **`frontend/app/contexts/HealthContext.tsx`**: Context لإدارة حالة النظام والخوادم المتعددة
- **`frontend/app/layout.tsx`**: دمج HealthProvider في التطبيق الرئيسي
- **`frontend/app/(dashboard)/layout.tsx`**: إضافة مؤشرات الحالة للوحة التحكم
- **`frontend/app/(dashboard)/statistics/page.tsx`**: إضافة لوحة حالة الخوادم
- **`frontend/app/page.tsx`**: إضافة مؤشر الحالة للصفحة الرئيسية

## كيفية العمل

### 1. عند فتح التطبيق
```typescript
// يتم عرض شاشة التحميل
<HealthLoadingScreen onHealthCheckComplete={handleComplete} />

// فحص سريع لجميع الخوادم
const healthStatus = await healthService.quickCheck();

// النظام يفحص:
// 1. الخادم المحلي (localhost:8000)
// 2. الخادم السحابي (render.com)
// 3. يختار الأسرع والأكثر استقراراً
```

### 2. أثناء الاستخدام
```typescript
// فحص دوري كل 30 ثانية لجميع الخوادم
useEffect(() => {
  const interval = setInterval(() => {
    checkHealth(); // يفحص جميع الخوادم ويختار الأفضل
  }, 30000);
  
  return () => clearInterval(interval);
}, []);

// في حالة فشل خادم، يتم التبديل تلقائياً للخادم الاحتياطي
```

### 3. عند وجود مشاكل
```typescript
// عرض تنبيه أحمر مع معلومات الخوادم
{hasError && (
  <div className="bg-red-50 border-b border-red-200">
    <p>تحذير: مشكلة في الاتصال بالخوادم</p>
    <button onClick={checkHealth}>إعادة فحص جميع الخوادم</button>
  </div>
)}

// عرض لوحة حالة الخوادم
<ServerStatusPanel />
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

## إدارة الخوادم المتعددة

### إضافة خادم جديد
```typescript
const healthService = new HealthService({
  servers: [
    'http://localhost:8000',
    'https://aziton-abo-nageb-backend.onrender.com',
    'https://your-new-server.com' // خادم جديد
  ]
});
```

### تبديل الخادم يدوياً
```typescript
const { switchToServer, availableServers } = useHealth();

// تبديل إلى خادم محدد
const success = await switchToServer('https://aziton-abo-nageb-backend.onrender.com');

// عرض الخوادم المتاحة
console.log('Available servers:', availableServers);
```

### أولوية الخوادم
```typescript
// النظام يختار الخادم حسب الأولوية:
// 1. الخوادم النشطة (isActive: true)
// 2. أقل وقت استجابة
// 3. الخادم المحلي له أولوية أعلى
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
import { HealthStatusIndicator, ServerStatusPanel } from '@/app/contexts/HealthContext';

// مؤشر بسيط
<HealthStatusIndicator showText={false} />

// مؤشر مع نص
<HealthStatusIndicator showText={true} />

// مؤشر مع معلومات الخادم
<HealthStatusIndicator showText={true} showServerInfo={true} />

// لوحة حالة الخوادم الكاملة
<ServerStatusPanel />
```

## الاستخدام في المكونات

### فحص الحالة يدوياً
```typescript
import { useHealth } from '@/app/contexts/HealthContext';

function MyComponent() {
  const { 
    healthStatus, 
    isChecking, 
    checkHealth, 
    activeServer, 
    availableServers 
  } = useHealth();
  
  if (healthStatus?.status === 'unhealthy') {
    return (
      <div>
        <p>جميع الخوادم غير متاحة حالياً</p>
        <button onClick={checkHealth}>إعادة المحاولة</button>
      </div>
    );
  }
  
  return (
    <div>
      <p>المحتوى العادي</p>
      <p>الخادم النشط: {activeServer}</p>
      <p>الخوادم المتاحة: {availableServers.length}</p>
    </div>
  );
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

## التحديثات الأخيرة (v2.0)

### ✅ تم إنجازه
- [x] دعم خوادم متعددة مع فحص متزامن
- [x] آلية تبديل تلقائي للخوادم
- [x] أولوية للخادم المحلي عند توافره
- [x] لوحة حالة الخوادم في صفحة الإحصائيات
- [x] مؤشرات حالة محسنة مع معلومات الخوادم
- [x] فحص متوازي لتحسين الأداء
- [x] دعم خادم Render.com كخادم احتياطي

## التحديثات المستقبلية

### ميزات مخططة
- [ ] إشعارات push عند وجود مشاكل
- [ ] إحصائيات مفصلة عن أوقات الاستجابة
- [ ] دعم لفحص خدمات خارجية
- [ ] لوحة تحكم للمراقبة
- [ ] دعم خوادم إضافية (AWS, DigitalOcean)
- [ ] موازنة التحميل الذكية

### تحسينات الأداء
- [x] فحص متوازي لخدمات متعددة
- [ ] تحسين خوارزمية إعادة المحاولة
- [ ] ضغط استجابات API
- [ ] تخزين مؤقت للاستجابات

---

## الدعم

للحصول على المساعدة أو الإبلاغ عن مشاكل، يرجى التواصل مع فريق التطوير.

**تم التطوير بـ ❤️ لضمان أفضل تجربة مستخدم**
