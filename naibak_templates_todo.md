# خطة تطوير قوالب نائبك - TODO List

## الهدف: تهيئة القوالب الموجودة بالبيانات الأساسية فقط (بدون مبالغة)

---

## المرحلة الأولى: الأساسيات السهلة جداً ⭐ (يوم واحد)

### 1. تحديث ملفات البيئة (.env) 
**الخدمات:** جميع الخدمات الـ 13

#### ✅ المطلوب:
- [ ] إضافة `SERVICE_NAME` لكل خدمة
- [ ] إضافة `DATABASE_NAME` مخصص لكل خدمة  
- [ ] إضافة `PORT` مخصص لكل خدمة
- [ ] إضافة `GOOGLE_CLOUD_PROJECT=naebak-472518`

#### 📝 مثال:
```env
# naibak-auth-service/.env
SERVICE_NAME=naibak-auth-service
DATABASE_NAME=naibak_auth_db
PORT=8001
GOOGLE_CLOUD_PROJECT=naebak-472518
```

---

### 2. تحديث ملف requirements.txt
**الخدمات:** جميع الخدمات

#### ✅ المطلوب:
- [ ] إضافة `psycopg2-binary` لـ PostgreSQL
- [ ] إضافة `redis` للتخزين المؤقت
- [ ] إضافة `gunicorn` للإنتاج
- [ ] إضافة `python-decouple` لمتغيرات البيئة

#### 📝 مثال:
```txt
Django==4.2.7
djangorestframework==3.14.0
psycopg2-binary==2.9.7
redis==4.6.0
gunicorn==21.2.0
python-decouple==3.8
```

---

### 3. إنشاء ملف settings/base.py موحد
**الخدمات:** جميع الخدمات

#### ✅ المطلوب:
- [ ] إعداد قاعدة البيانات PostgreSQL
- [ ] إعداد Redis للتخزين المؤقت
- [ ] إعداد CORS للواجهة الأمامية
- [ ] إعداد المنطقة الزمنية لمصر

#### 📝 مثال:
```python
from decouple import config

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('DATABASE_NAME'),
        'USER': config('DB_USER', default='postgres'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST', default='localhost'),
        'PORT': config('DB_PORT', default='5432'),
    }
}

TIME_ZONE = 'Africa/Cairo'
LANGUAGE_CODE = 'ar'
```

---

## المرحلة الثانية: البيانات الأساسية ⭐⭐ (يومين)

### 4. إضافة المحافظات المصرية
**الخدمات:** naibak-auth-service, naibak-content-service

#### ✅ المطلوب:
- [ ] إنشاء model `Governorate`
- [ ] إنشاء fixture بالمحافظات الـ 27
- [ ] إضافة management command لتحميل البيانات

#### 📝 مثال:
```python
# models.py
class Governorate(models.Model):
    name = models.CharField(max_length=50)
    name_en = models.CharField(max_length=50)
    code = models.CharField(max_length=3, unique=True)
```

---

### 5. نماذج المستخدمين الأساسية
**الخدمة:** naibak-auth-service فقط

#### ✅ المطلوب:
- [ ] تحديث User model بالحقول المطلوبة
- [ ] إضافة UserProfile model
- [ ] إنشاء serializers أساسية

#### 📝 مثال:
```python
class User(AbstractUser):
    user_type = models.CharField(
        max_length=20,
        choices=[('citizen', 'مواطن'), ('candidate', 'مرشح')]
    )
    national_id = models.CharField(max_length=14, unique=True)
    governorate = models.ForeignKey(Governorate, on_delete=models.CASCADE)
    phone = models.CharField(max_length=15)
```

---

### 6. APIs الأساسية للمصادقة
**الخدمة:** naibak-auth-service فقط

#### ✅ المطلوب:
- [ ] endpoint للتسجيل `/api/auth/register/`
- [ ] endpoint لتسجيل الدخول `/api/auth/login/`
- [ ] endpoint لتسجيل الخروج `/api/auth/logout/`
- [ ] endpoint للملف الشخصي `/api/auth/profile/`

---

## المرحلة الثالثة: التخصص البسيط ⭐⭐⭐ (3 أيام)

### 7. خدمة عداد الزوار
**الخدمة:** naibak-visitor-counter-service

#### ✅ المطلوب:
- [ ] model `VisitorCount` بسيط
- [ ] endpoint `/api/visitors/count/` للعرض
- [ ] endpoint `/api/visitors/increment/` للزيادة
- [ ] استخدام Redis للتخزين السريع

---

### 8. خدمة الإشعارات الأساسية
**الخدمة:** naibak-notifications-service

#### ✅ المطلوب:
- [ ] model `Notification` بسيط
- [ ] endpoint `/api/notifications/` للعرض
- [ ] endpoint `/api/notifications/{id}/read/` للقراءة
- [ ] endpoint `/api/notifications/unread-count/` للعدد

---

### 9. خدمة الأخبار البسيطة
**الخدمة:** naibak-news-service

#### ✅ المطلوب:
- [ ] model `NewsItem` بسيط
- [ ] endpoint `/api/news/current/` للخبر الحالي
- [ ] endpoint `/api/news/` لجميع الأخبار
- [ ] admin interface لإدارة الأخبار

---

### 10. خدمة البنرات
**الخدمة:** naibak-banner-service

#### ✅ المطلوب:
- [ ] model `Banner` بسيط
- [ ] endpoint `/api/banners/current/` للبانر الحالي
- [ ] endpoint `/api/banners/` لجميع البنرات
- [ ] حقل `is_active` للتفعيل/الإلغاء

---

## المرحلة الرابعة: الخدمات المتوسطة ⭐⭐⭐⭐ (أسبوع)

### 11. خدمة الرسائل
**الخدمة:** naibak-messaging-service

#### ✅ المطلوب:
- [ ] model `Message` مع sender/recipient
- [ ] endpoint `/api/messages/` للإرسال والاستقبال
- [ ] endpoint `/api/messages/{id}/` لرسالة محددة
- [ ] فلترة حسب المرسل/المستقبل

---

### 12. خدمة الشكاوى
**الخدمة:** naibak-complaints-service

#### ✅ المطلوب:
- [ ] model `Complaint` أساسي
- [ ] endpoint `/api/complaints/` للإرسال والعرض
- [ ] دعم رفع ملف واحد فقط
- [ ] حالات بسيطة (جديد، قيد المراجعة، مكتمل)

---

### 13. خدمة التقييمات
**الخدمة:** naibak-ratings-service

#### ✅ المطلوب:
- [ ] model `Rating` بسيط (1-5 نجوم)
- [ ] endpoint `/api/ratings/` للتقييم
- [ ] endpoint `/api/ratings/candidate/{id}/` لتقييمات مرشح
- [ ] حساب المتوسط البسيط

---

## المرحلة الخامسة: الخدمات المساعدة ⭐⭐ (يومين)

### 14. خدمة الإحصائيات البسيطة
**الخدمة:** naibak-statistics-service

#### ✅ المطلوب:
- [ ] عدد المستخدمين المسجلين
- [ ] عدد الرسائل المرسلة
- [ ] عدد الشكاوى المقدمة
- [ ] endpoint `/api/statistics/overall/` فقط

---

### 15. خدمة الإدارة الأساسية
**الخدمة:** naibak-admin-service

#### ✅ المطلوب:
- [ ] model `SocialLink` للروابط الاجتماعية
- [ ] endpoint `/api/admin/social-links/` للعرض والتحديث
- [ ] 5 روابط ثابتة (Facebook, Twitter, Instagram, YouTube, LinkedIn)

---

### 16. خدمة الألوان البسيطة
**الخدمة:** naibak-theme-service

#### ✅ المطلوب:
- [ ] model `ThemeSettings` بسيط
- [ ] 3 ألوان فقط (primary, secondary, accent)
- [ ] endpoint `/api/theme/current/` للعرض
- [ ] endpoint `/api/theme/` للتحديث

---

## المرحلة السادسة: النشر والاختبار ⭐⭐⭐⭐⭐ (3 أيام)

### 17. إعداد Docker لكل خدمة
#### ✅ المطلوب:
- [ ] Dockerfile بسيط لكل خدمة
- [ ] docker-compose.yml للتطوير المحلي
- [ ] متغيرات البيئة في Docker

---

### 18. GitHub Actions أساسية
#### ✅ المطلوب:
- [ ] workflow للبناء والاختبار
- [ ] workflow للنشر على Cloud Run
- [ ] اختبار واحد فقط لكل خدمة

---

### 19. اختبار التكامل البسيط
#### ✅ المطلوب:
- [ ] اختبار الاتصال بين الخدمات
- [ ] اختبار الواجهة الأمامية مع الخدمات
- [ ] توثيق APIs بسيط

---

## التقدير الزمني الإجمالي: 3 أسابيع

- **الأسبوع الأول:** المراحل 1-3 (الأساسيات)
- **الأسبوع الثاني:** المراحل 4-5 (الخدمات الأساسية)  
- **الأسبوع الثالث:** المرحلة 6 (النشر والاختبار)

---

## ملاحظات مهمة:

### ✅ ما سيتم عمله:
- بيانات أساسية حقيقية ومفيدة
- APIs تعمل فعلياً
- نشر حقيقي على Google Cloud
- تكامل بسيط بين الخدمات

### ❌ ما لن يتم عمله (لتجنب المبالغة):
- واجهات إدارة معقدة
- أنظمة أمان متقدمة
- تحليلات معقدة
- ميزات متقدمة غير ضرورية

---

## الأولوية في التنفيذ:

1. **عالية جداً:** المراحل 1-2 (ضرورية للعمل)
2. **عالية:** المراحل 3-4 (مطلوبة للوظائف الأساسية)
3. **متوسطة:** المرحلة 5 (مفيدة للمستخدمين)
4. **منخفضة:** المرحلة 6 (للنشر والإنتاج)

**هل تريد البدء بالمرحلة الأولى الآن؟** 🚀
