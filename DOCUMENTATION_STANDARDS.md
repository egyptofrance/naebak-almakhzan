# 📚 معايير التوثيق الموحدة لمشروع نائبك

**تاريخ الإنشاء:** 26 سبتمبر 2025  
**آخر تحديث:** 26 سبتمبر 2025  
**الإصدار:** 1.0  
**المؤلف:** فريق تطوير نائبك  

---

## 🎯 **الهدف من هذا المستند**

هذا المستند يحدد المعايير الموحدة للتوثيق عبر جميع الخدمات المصغرة الـ 15 في مشروع نائبك. الهدف هو ضمان التناسق والجودة في التوثيق، مما يسهل على المطورين فهم النظام والمساهمة فيه بفعالية.

---

## 📋 **نطاق التطبيق**

هذه المعايير تنطبق على جميع الخدمات المصغرة في مشروع نائبك:

### **الخدمات المتقدمة (Django):**
- naebak-auth-service
- naebak-complaints-service  
- naebak-admin-service
- naebak-ratings-service
- naebak-news-service
- naebak-banner-service

### **الخدمات قيد التطوير:**
- naebak-gateway
- naebak-frontend (Next.js)
- naebak-admin-frontend (Next.js)

### **الخدمات الأولية (Flask):**
- naebak-messaging-service
- naebak-notifications-service
- naebak-content-service
- naebak-visitor-counter-service
- naebak-statistics-service
- naebak-theme-service

---

## 📖 **1. معايير ملفات README**

### **1.1 الهيكل المطلوب**

كل مستودع يجب أن يحتوي على ملف `README.md` يتبع الهيكل التالي:

```markdown
# 🏷️ اسم الخدمة

## 📝 الوصف
وصف مختصر وواضح للخدمة ووظيفتها

## ✨ الميزات الرئيسية
- قائمة بالميزات الأساسية
- استخدام نقاط واضحة ومحددة

## 🛠️ التقنيات المستخدمة
- إطار العمل (Django/Flask/Next.js)
- قاعدة البيانات
- أدوات إضافية

## 🚀 التثبيت والتشغيل
خطوات واضحة للتثبيت والتشغيل المحلي

## 📚 توثيق الـ API
رابط لتوثيق الـ API المفصل

## 🧪 الاختبارات
كيفية تشغيل الاختبارات

## 🚀 النشر
إرشادات النشر على Google Cloud Run

## 🤝 المساهمة
إرشادات للمطورين الجدد

## 📄 الترخيص
معلومات الترخيص
```

### **1.2 معايير الكتابة**

- **اللغة:** استخدام اللغة العربية للوصف العام والإنجليزية للأكواد
- **الوضوح:** كل قسم يجب أن يكون واضحاً ومفهوماً
- **التحديث:** يجب تحديث README عند كل تغيير مهم
- **الأمثلة:** تضمين أمثلة عملية للاستخدام

---

## 🔌 **2. معايير توثيق الـ API**

### **2.1 للخدمات Django**

#### **أدوات التوثيق المطلوبة:**
- **drf-spectacular** للتوثيق التلقائي
- **Swagger UI** للواجهة التفاعلية
- **Redoc** كبديل للعرض

#### **متطلبات التوثيق:**
```python
# في كل view يجب إضافة:
from drf_spectacular.utils import extend_schema, OpenApiParameter

@extend_schema(
    summary="وصف مختصر للـ endpoint",
    description="وصف مفصل للوظيفة",
    parameters=[
        OpenApiParameter(
            name='parameter_name',
            description='وصف المعامل',
            required=True,
            type=str
        )
    ],
    responses={
        200: "نجح الطلب",
        400: "خطأ في البيانات المرسلة",
        401: "غير مخول",
        404: "غير موجود"
    },
    tags=['اسم المجموعة']
)
```

### **2.2 للخدمات Flask**

#### **أدوات التوثيق المطلوبة:**
- **Flask-RESTX** للتوثيق التلقائي
- **Swagger UI** المدمج

#### **متطلبات التوثيق:**
```python
# في كل endpoint يجب إضافة:
@api.doc('وصف_الوظيفة')
@api.expect(input_model)
@api.marshal_with(output_model)
def endpoint_function():
    """
    وصف مفصل للـ endpoint
    ---
    parameters:
      - name: parameter_name
        description: وصف المعامل
        required: true
        type: string
    responses:
      200:
        description: نجح الطلب
      400:
        description: خطأ في البيانات
    """
```

### **2.3 للخدمات Next.js**

#### **أدوات التوثيق المطلوبة:**
- **TypeDoc** للمكونات
- **Storybook** للـ UI components
- **JSDoc** للتعليقات

#### **متطلبات التوثيق:**
```typescript
/**
 * مكون لعرض معلومات المستخدم
 * @param props - خصائص المكون
 * @param props.user - بيانات المستخدم
 * @param props.onEdit - دالة التعديل
 * @returns JSX.Element
 */
interface UserCardProps {
  user: User;
  onEdit: (user: User) => void;
}

const UserCard: React.FC<UserCardProps> = ({ user, onEdit }) => {
  // تنفيذ المكون
};
```

---

## 💬 **3. معايير تعليقات الكود**

### **3.1 للكود Python (Django/Flask)**

#### **Docstrings للكلاسات:**
```python
class UserService:
    """
    خدمة إدارة المستخدمين
    
    هذه الكلاس تحتوي على جميع العمليات المتعلقة بإدارة المستخدمين
    في النظام، بما في ذلك التسجيل والمصادقة وإدارة الصلاحيات.
    
    Attributes:
        db_session: جلسة قاعدة البيانات
        cache_service: خدمة التخزين المؤقت
    """
```

#### **Docstrings للدوال:**
```python
def create_user(self, user_data: dict) -> User:
    """
    إنشاء مستخدم جديد في النظام
    
    Args:
        user_data (dict): بيانات المستخدم الجديد
            - email (str): البريد الإلكتروني
            - password (str): كلمة المرور
            - user_type (str): نوع المستخدم
    
    Returns:
        User: كائن المستخدم المُنشأ
    
    Raises:
        ValidationError: في حالة بيانات غير صحيحة
        DuplicateEmailError: في حالة وجود البريد مسبقاً
    
    Example:
        >>> user_service = UserService()
        >>> user = user_service.create_user({
        ...     'email': 'user@example.com',
        ...     'password': 'secure_password',
        ...     'user_type': 'citizen'
        ... })
    """
```

### **3.2 للكود TypeScript/JavaScript**

#### **تعليقات المكونات:**
```typescript
/**
 * مكون عرض قائمة الشكاوى
 * 
 * يعرض هذا المكون قائمة بجميع الشكاوى مع إمكانية الفلترة والبحث.
 * يدعم التحديث التلقائي والتنقل بين الصفحات.
 * 
 * @component
 * @example
 * ```tsx
 * <ComplaintsList 
 *   filters={{ status: 'pending' }}
 *   onComplaintClick={handleComplaintClick}
 * />
 * ```
 */
```

#### **تعليقات الدوال:**
```typescript
/**
 * جلب قائمة الشكاوى من الخادم
 * 
 * @param filters - فلاتر البحث
 * @param page - رقم الصفحة
 * @param limit - عدد العناصر في الصفحة
 * @returns Promise<ComplaintsResponse> - قائمة الشكاوى مع معلومات التنقل
 * 
 * @throws {ApiError} عند فشل الطلب
 * @throws {ValidationError} عند بيانات فلترة غير صحيحة
 */
async function fetchComplaints(
  filters: ComplaintFilters,
  page: number = 1,
  limit: number = 10
): Promise<ComplaintsResponse> {
  // تنفيذ الدالة
}
```

---

## 🏗️ **4. معايير سجلات قرارات المعمارية (ADRs)**

### **4.1 هيكل ملف ADR**

كل قرار معماري يجب توثيقه في ملف منفصل باستخدام الهيكل التالي:

```markdown
# ADR-001: اختيار Django REST Framework للخدمات الخلفية

## الحالة
مقبول

## السياق
نحتاج لاختيار إطار عمل لتطوير APIs للخدمات الخلفية في مشروع نائبك.

## القرار
استخدام Django REST Framework (DRF) لجميع الخدمات الخلفية.

## العواقب

### الإيجابية:
- توثيق تلقائي ممتاز
- نظام مصادقة وتخويل قوي
- مجتمع كبير ودعم واسع

### السلبية:
- منحنى تعلم أطول للمطورين الجدد
- استهلاك ذاكرة أعلى من Flask

## البدائل المدروسة
- FastAPI
- Flask-RESTful
- Express.js

## تاريخ القرار
2025-09-26

## المراجعون
- كبير المهندسين
- فريق التطوير الخلفي
```

### **4.2 تسمية ملفات ADR**

- **التنسيق:** `ADR-XXX-short-title.md`
- **الترقيم:** متسلسل بدءاً من 001
- **المكان:** مجلد `docs/adrs/` في كل مستودع

---

## 📅 **5. معايير سجلات التغيير (Changelogs)**

### **5.1 هيكل ملف CHANGELOG**

```markdown
# سجل التغييرات

جميع التغييرات المهمة في هذا المشروع سيتم توثيقها في هذا الملف.

التنسيق مبني على [Keep a Changelog](https://keepachangelog.com/ar/1.0.0/)،
وهذا المشروع يتبع [Semantic Versioning](https://semver.org/lang/ar/).

## [غير منشور]

### مضاف
- ميزة جديدة قيد التطوير

### تم تغييره
- تحسين في الأداء

### مُصلح
- إصلاح خطأ في المصادقة

## [1.2.0] - 2025-09-26

### مضاف
- إضافة نظام الإشعارات الفورية
- دعم تحميل الملفات في الشكاوى

### تم تغييره
- تحسين واجهة المستخدم للوحة التحكم
- تحديث مكتبات الأمان

### مُصلح
- إصلاح مشكلة في تسجيل الدخول
- حل مشكلة بطء تحميل الصفحات

### أُزيل
- إزالة API قديم غير مستخدم
```

### **5.2 قواعد التحديث**

- **تحديث فوري:** عند كل إصدار جديد
- **التصنيف:** استخدام فئات واضحة (مضاف، تم تغييره، مُصلح، أُزيل)
- **التواريخ:** تنسيق ISO (YYYY-MM-DD)
- **الروابط:** ربط كل إصدار بـ tag في Git

---

## 🛠️ **6. أدوات التوثيق المطلوبة**

### **6.1 للخدمات Django**

```python
# requirements.txt
drf-spectacular==0.26.5
django-cors-headers==4.3.1
django-extensions==3.2.3

# settings.py
INSTALLED_APPS = [
    # ...
    'drf_spectacular',
    'corsheaders',
]

REST_FRAMEWORK = {
    'DEFAULT_SCHEMA_CLASS': 'drf_spectacular.openapi.AutoSchema',
}

SPECTACULAR_SETTINGS = {
    'TITLE': 'Naebak API',
    'DESCRIPTION': 'API documentation for Naebak platform',
    'VERSION': '1.0.0',
    'SERVE_INCLUDE_SCHEMA': False,
    'COMPONENT_SPLIT_REQUEST': True,
}
```

### **6.2 للخدمات Flask**

```python
# requirements.txt
flask-restx==1.2.0
flask-cors==4.0.0

# app.py
from flask import Flask
from flask_restx import Api
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

api = Api(
    app,
    version='1.0',
    title='Naebak Service API',
    description='API documentation for Naebak microservice',
    doc='/docs/'
)
```

### **6.3 للخدمات Next.js**

```json
// package.json
{
  "devDependencies": {
    "@storybook/react": "^7.4.0",
    "typedoc": "^0.25.0",
    "jsdoc": "^4.0.0"
  }
}
```

```typescript
// typedoc.json
{
  "entryPoints": ["./src"],
  "out": "./docs",
  "theme": "default",
  "includeVersion": true,
  "excludePrivate": true,
  "excludeProtected": true,
  "excludeExternals": true
}
```

---

## 📏 **7. معايير الجودة والمراجعة**

### **7.1 قائمة مراجعة التوثيق**

قبل نشر أي تحديث، يجب التأكد من:

- [ ] **README محدث** مع آخر التغييرات
- [ ] **API documentation** يعكس جميع endpoints
- [ ] **Code comments** موجودة للكود المعقد
- [ ] **ADR مكتوب** لأي قرار معماري جديد
- [ ] **CHANGELOG محدث** مع التغييرات الجديدة
- [ ] **أمثلة عملية** متوفرة للاستخدام
- [ ] **روابط صحيحة** وتعمل بشكل سليم

### **7.2 معايير المراجعة**

#### **الوضوح:**
- هل التوثيق واضح ومفهوم؟
- هل يمكن لمطور جديد فهم الكود من التوثيق؟

#### **الاكتمال:**
- هل جميع الوظائف موثقة؟
- هل الأمثلة شاملة وعملية؟

#### **الدقة:**
- هل التوثيق يطابق الكود الفعلي؟
- هل المعلومات محدثة؟

#### **التناسق:**
- هل التوثيق يتبع المعايير الموحدة؟
- هل التنسيق متسق عبر المشروع؟

---

## 🔄 **8. عملية التحديث والصيانة**

### **8.1 جدولة التحديث**

- **يومياً:** مراجعة code comments للكود الجديد
- **أسبوعياً:** تحديث README إذا لزم الأمر
- **شهرياً:** مراجعة شاملة لجميع التوثيق
- **عند كل إصدار:** تحديث CHANGELOG و API docs

### **8.2 المسؤوليات**

| الدور | المسؤولية |
|-------|-----------|
| **المطور** | كتابة code comments وتحديث README |
| **قائد الفريق** | مراجعة ADRs والموافقة عليها |
| **مهندس DevOps** | صيانة أدوات التوثيق التلقائي |
| **مدير المنتج** | مراجعة user guides والتوثيق العام |

---

## 📞 **9. الدعم والمساعدة**

### **9.1 للحصول على المساعدة**

- **GitHub Issues:** للمشاكل التقنية في التوثيق
- **فريق التطوير:** للاستفسارات حول المعايير
- **قناة Slack:** للنقاشات السريعة

### **9.2 تحديث هذا المستند**

هذا المستند قابل للتطوير والتحديث. لاقتراح تحسينات:

1. إنشاء Issue في مستودع almakhzan
2. وصف التحسين المقترح بوضوح
3. تقديم أمثلة إذا أمكن
4. انتظار مراجعة الفريق والموافقة

---

## 📚 **10. مراجع ومصادر إضافية**

- [Django REST Framework Documentation](https://www.django-rest-framework.org/)
- [Flask-RESTX Documentation](https://flask-restx.readthedocs.io/)
- [TypeDoc Documentation](https://typedoc.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Semantic Versioning](https://semver.org/)
- [Architecture Decision Records](https://adr.github.io/)

---

**تاريخ آخر مراجعة:** 26 سبتمبر 2025  
**الإصدار:** 1.0  
**الحالة:** نشط ومعتمد
