# نظام إدارة المدرسة
## تصميم قاعدة البيانات (Database Design)

**إصدار المستند:** 1.0  
**التاريخ:** أكتوبر 2026  
**الحالة:** تصميم مقترح (Proposed Design)  
**نوع قاعدة البيانات:** قاعدة بيانات علائقية (Relational Database)  
**نوع التطبيق:** تطبيق سطح مكتب (Desktop Application)

---

## 1. الغرض من المستند (Purpose)

يحدد هذا المستند التصميم المنطقي المقترح لقاعدة بيانات نظام إدارة المدرسة (School Management System – SMS).

ويوضح الكيانات الرئيسية (Entities)، والخصائص (Attributes)، والعلاقات (Relationships)، والمفاتيح (Keys)، وقيود سلامة البيانات (Integrity Constraints) اللازمة لدعم الوحدات الوظيفية السبع للنظام.

ويوفر التصميم أساسًا مستقلًا عن تقنيات التنفيذ، يمكن الاعتماد عليه عند إنشاء قاعدة البيانات ومخطط الكيانات والعلاقات (Entity Relationship Diagram – ERD).

## 2. نظرة عامة على تصميم قاعدة البيانات (Database Design Overview)

يستخدم نظام إدارة المدرسة قاعدة بيانات علائقية مركزية (Centralized Relational Database)، تشترك في استخدامها أجهزة سطح المكتب المصرح لها من خلال الشبكة المحلية للمدرسة (LAN).

صُممت قاعدة البيانات لتحقيق الأهداف التالية:

- المحافظة على اتساق السجلات المؤسسية.
- دعم عدة مستخدمين مصرح لهم بالعمل على النظام.
- تقليل التكرار غير الضروري للبيانات.
- الاحتفاظ بالمعلومات الأكاديمية والمالية التاريخية.
- فرض التكامل المرجعي (Referential Integrity).
- دعم المعاملات الآمنة والموثوقة.

صُمم المخطط المقترح وفق مبادئ **الصيغة العادية الثالثة (Third Normal Form – 3NF)** من الناحية المنطقية، على أن يتم التحقق من ذلك أثناء التنفيذ الفعلي لقاعدة البيانات.

## 3. تحديد الكيانات (Entity Identification)

تُنظم قاعدة البيانات ضمن سبعة مجالات منطقية.

| المجال | الكيانات |
|---|---|
| إدارة الطلاب (Student Management) | Student, Enrollment |
| إدارة المعلمين (Teacher Management) | Teacher, TeachingAssignment |
| إدارة الصفوف والشعب (Class Management) | AcademicYear, GradeLevel, ClassSection, Subject |
| إدارة الدرجات (Grade Management) | AssessmentComponent, StudentGrade, GradeFinalization |
| إدارة الرسوم الدراسية (Fee Management) | FeeStructure, StudentCharge, Payment, FinancialAdjustment |
| إدارة المستخدمين (User Management) | UserAccount, Role, Permission, UserRole, RolePermission |
| تدقيق النظام (System Auditing) | AuditLog |

**إجمالي الكيانات المقترحة: 19 كيانًا.**

## 4. تعريف الكيانات (Entity Definitions)

### 4.1 إدارة الطلاب (Student Management)

#### الطالب (Student)

يُستخدم لتخزين بيانات هوية الطالب ومعلوماته الشخصية.

| الخاصية (Attribute) | المفتاح (Key) | الوصف |
|---|---|---|
| StudentID | PK | معرف فريد للطالب. |
| StudentNumber | UNIQUE | الرقم المدرسي الخاص بالطالب. |
| FirstName | — | الاسم الأول للطالب. |
| LastName | — | اسم العائلة للطالب. |
| DateOfBirth | — | تاريخ ميلاد الطالب. |
| Gender | — | الجنس المسجل للطالب. |
| ContactInformation | — | معلومات التواصل الخاصة بالطالب. |
| StudentStatus | — | الحالة المؤسسية الحالية للطالب. |
| CreatedAt | — | تاريخ ووقت إنشاء السجل. |

#### القيد الدراسي (Enrollment)

يُستخدم لتخزين معلومات قيد الطلاب بحسب السنة الدراسية.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| EnrollmentID | PK | معرف فريد للقيد الدراسي. |
| StudentID | FK | مفتاح خارجي يشير إلى Student. |
| AcademicYearID | FK | مفتاح خارجي يشير إلى AcademicYear. |
| ClassSectionID | FK | مفتاح خارجي يشير إلى ClassSection. |
| EnrollmentDate | — | تاريخ تسجيل القيد الدراسي. |
| EnrollmentStatus | — | حالة القيد الدراسي. |
| EffectiveFrom | — | تاريخ بداية سريان التوزيع. |
| EffectiveTo | — | تاريخ انتهاء سريان التوزيع، إن وجد. |

**القيد (Constraint):** لا يجوز أن يمتلك الطالب أكثر من قيد دراسي نشط واحد خلال السنة الدراسية نفسها.

### 4.2 إدارة المعلمين (Teacher Management)

#### المعلم (Teacher)

يُستخدم لتخزين الملفات التعريفية للمعلمين.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| TeacherID | PK | معرف فريد للمعلم. |
| TeacherNumber | UNIQUE | الرقم المؤسسي للمعلم. |
| FirstName | — | الاسم الأول للمعلم. |
| LastName | — | اسم العائلة للمعلم. |
| ContactInformation | — | معلومات التواصل الخاصة بالمعلم. |
| TeacherStatus | — | الحالة الوظيفية الحالية للمعلم. |
| CreatedAt | — | تاريخ ووقت إنشاء السجل. |

#### التكليف التدريسي (TeachingAssignment)

يربط المعلمين بالمواد والشعب الدراسية.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| AssignmentID | PK | معرف فريد للتكليف التدريسي. |
| TeacherID | FK | يشير إلى Teacher. |
| SubjectID | FK | يشير إلى Subject. |
| ClassSectionID | FK | يشير إلى ClassSection. |
| AcademicYearID | FK | يشير إلى AcademicYear. |
| AssignmentStatus | — | حالة التكليف التدريسي. |

**القيد:** يُمنع وجود تكليفات تدريسية نشطة مكررة للمعلم نفسه والمادة نفسها والشعبة نفسها والسنة الدراسية نفسها.

### 4.3 إدارة الصفوف والشعب (Class Management)

#### السنة الدراسية (AcademicYear)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| AcademicYearID | PK | معرف فريد للسنة الدراسية. |
| YearName | UNIQUE | اسم أو تسمية السنة الدراسية. |
| StartDate | — | تاريخ بداية السنة الدراسية. |
| EndDate | — | تاريخ نهاية السنة الدراسية. |
| YearStatus | — | حالة السنة الدراسية. |

#### الصف الدراسي (GradeLevel)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| GradeLevelID | PK | معرف فريد للصف الدراسي. |
| GradeName | UNIQUE | اسم الصف الدراسي. |
| GradeOrder | — | ترتيب الصف ضمن التسلسل الدراسي. |

#### الشعبة الدراسية (ClassSection)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| ClassSectionID | PK | معرف فريد للشعبة الدراسية. |
| AcademicYearID | FK | يشير إلى AcademicYear. |
| GradeLevelID | FK | يشير إلى GradeLevel. |
| SectionName | — | اسم أو رمز الشعبة. |
| SectionStatus | — | الحالة الحالية للشعبة. |

**القيد:** يجب أن تكون أسماء الشعب فريدة ضمن كل سنة دراسية وصف دراسي.

#### المادة الدراسية (Subject)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| SubjectID | PK | معرف فريد للمادة الدراسية. |
| SubjectCode | UNIQUE | الرمز المؤسسي للمادة الدراسية. |
| SubjectName | — | اسم المادة الدراسية. |
| SubjectStatus | — | حالة إتاحة المادة الدراسية. |

### 4.4 إدارة الدرجات (Grade Management)

#### مكوّن التقييم (AssessmentComponent)

يحدد مكونات التقييم والحدود المسموح بها للدرجات.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| AssessmentComponentID | PK | معرف فريد لمكوّن التقييم. |
| SubjectID | FK | يشير إلى Subject. |
| AcademicYearID | FK | يشير إلى AcademicYear. |
| ComponentName | — | اسم مكوّن التقييم. |
| MaximumScore | — | الحد الأعلى المسموح به للدرجة. |
| Weight | — | وزن المكوّن في احتساب الدرجة النهائية. |

#### درجة الطالب (StudentGrade)

يُستخدم لتخزين نتائج تقييم الطلاب بصورة فردية.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| StudentGradeID | PK | معرف فريد لسجل الدرجة. |
| EnrollmentID | FK | يشير إلى Enrollment. |
| AssessmentComponentID | FK | يشير إلى AssessmentComponent. |
| Score | — | الدرجة المسجلة للطالب. |
| GradeStatus | — | حالة الدرجة: أولية أو معتمدة نهائيًا. |
| RecordedBy | FK | يشير إلى UserAccount. |
| RecordedAt | — | تاريخ ووقت إدخال الدرجة. |

**القيد:** يجب أن تقع الدرجات المسجلة ضمن حدود التقييم المحددة.

#### اعتماد الدرجات (GradeFinalization)

يُستخدم لتتبع قرارات اعتماد الدرجات نهائيًا.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| FinalizationID | PK | معرف فريد لعملية الاعتماد. |
| ClassSectionID | FK | يشير إلى ClassSection. |
| SubjectID | FK | يشير إلى Subject. |
| AcademicYearID | FK | يشير إلى AcademicYear. |
| FinalizedBy | FK | يشير إلى UserAccount. |
| FinalizedAt | — | تاريخ ووقت اعتماد الدرجات. |
| FinalizationStatus | — | الحالة الحالية لعملية الاعتماد. |

### 4.5 إدارة الرسوم الدراسية (Fee Management)

#### هيكل الرسوم (FeeStructure)

يحدد الرسوم الدراسية بحسب السنة الدراسية والصف الدراسي.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| FeeStructureID | PK | معرف فريد لهيكل الرسوم. |
| AcademicYearID | FK | يشير إلى AcademicYear. |
| GradeLevelID | FK | يشير إلى GradeLevel. |
| FeeName | — | اسم الرسم الدراسي. |
| FeeAmount | — | مبلغ الرسم المعتمد. |
| FeeStatus | — | حالة هيكل الرسوم. |

#### الرسوم المستحقة على الطالب (StudentCharge)

يُستخدم لتخزين الرسوم المفروضة على الطلاب.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| ChargeID | PK | معرف فريد للرسم المستحق. |
| EnrollmentID | FK | يشير إلى Enrollment. |
| FeeStructureID | FK | يشير إلى FeeStructure. |
| ChargeAmount | — | المبلغ المستحق. |
| ChargeDate | — | تاريخ فرض الرسم. |
| ChargeStatus | — | حالة الرسم المستحق. |

#### المدفوعات (Payment)

يُستخدم لتخزين معاملات الدفع الخاصة بالطلاب.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| PaymentID | PK | معرف فريد لعملية الدفع. |
| PaymentNumber | UNIQUE | الرقم المؤسسي للمعاملة المالية. |
| EnrollmentID | FK | يشير إلى Enrollment. |
| PaymentAmount | — | المبلغ المستلم. |
| PaymentDate | — | تاريخ الدفع. |
| PaymentStatus | — | حالة المعاملة المالية. |
| RecordedBy | FK | يشير إلى UserAccount. |
| RecordedAt | — | تاريخ ووقت تسجيل المعاملة. |

#### التعديل المالي (FinancialAdjustment)

يُستخدم لتخزين التصحيحات والتعديلات المالية المعتمدة.

| الخاصية | المفتاح | الوصف |
|---|---|---|
| AdjustmentID | PK | معرف فريد للتعديل المالي. |
| EnrollmentID | FK | يشير إلى Enrollment. |
| PaymentID | FK, NULL | يشير إلى Payment عند وجود معاملة دفع مرتبطة. |
| AdjustmentAmount | — | قيمة التعديل المالي، ويمكن أن تكون موجبة أو سالبة. |
| AdjustmentReason | — | المبرر الموثق للتعديل. |
| ApprovedBy | FK | يشير إلى UserAccount. |
| CreatedAt | — | تاريخ ووقت إنشاء التعديل. |

**القيد:** يجب أن تحافظ عمليات التصحيح المالي على معاملات الدفع الأصلية دون حذفها.

### 4.6 إدارة المستخدمين (User Management)

#### حساب المستخدم (UserAccount)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| UserID | PK | معرف فريد للمستخدم. |
| Username | UNIQUE | اسم المستخدم المستخدم لتسجيل الدخول. |
| PasswordHash | — | القيمة المجزأة لكلمة المرور والمخزنة بصورة آمنة. |
| AccountStatus | — | حالة الحساب: نشط أو غير نشط. |
| CreatedAt | — | تاريخ ووقت إنشاء الحساب. |

#### الدور (Role)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| RoleID | PK | معرف فريد للدور. |
| RoleName | UNIQUE | اسم الدور المؤسسي. |
| RoleDescription | — | وصف مسؤوليات الدور. |

#### الصلاحية (Permission)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| PermissionID | PK | معرف فريد للصلاحية. |
| PermissionCode | UNIQUE | الرمز البرمجي للصلاحية. |
| PermissionDescription | — | وصف العملية المسموح بتنفيذها. |

#### ربط المستخدم بالأدوار (UserRole)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| UserID | PK, FK | يشير إلى UserAccount. |
| RoleID | PK, FK | يشير إلى Role. |

#### ربط الأدوار بالصلاحيات (RolePermission)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| RoleID | PK, FK | يشير إلى Role. |
| PermissionID | PK, FK | يشير إلى Permission. |

### 4.7 تدقيق النظام (System Auditing)

#### سجل التدقيق (AuditLog)

| الخاصية | المفتاح | الوصف |
|---|---|---|
| AuditID | PK | معرف فريد لسجل التدقيق. |
| UserID | FK | يشير إلى UserAccount. |
| ActionType | — | نوع العملية المنفذة. |
| EntityName | — | اسم الكيان المتأثر. |
| RecordIdentifier | — | معرف السجل الذي تأثر بالعملية. |
| OldValue | — | القيمة السابقة، عند الحاجة. |
| NewValue | — | القيمة الجديدة، عند الحاجة. |
| EventTimestamp | — | تاريخ ووقت وقوع الحدث. |

يجب حماية سجلات التدقيق من التعديل غير المصرح به.

## 5. العلاقات بين الكيانات (Entity Relationships)

يلخص الجدول التالي العلاقات الرئيسية بين الكيانات.

| الكيان الأساسي (Parent Entity) | الكيان المرتبط (Child Entity) | نوع العلاقة (Cardinality) |
|---|---|---|
| Student | Enrollment | 1:N |
| AcademicYear | Enrollment | 1:N |
| ClassSection | Enrollment | 1:N |
| Teacher | TeachingAssignment | 1:N |
| Subject | TeachingAssignment | 1:N |
| ClassSection | TeachingAssignment | 1:N |
| AcademicYear | TeachingAssignment | 1:N |
| AcademicYear | ClassSection | 1:N |
| GradeLevel | ClassSection | 1:N |
| Subject | AssessmentComponent | 1:N |
| AcademicYear | AssessmentComponent | 1:N |
| Enrollment | StudentGrade | 1:N |
| AssessmentComponent | StudentGrade | 1:N |
| ClassSection | GradeFinalization | 1:N |
| Subject | GradeFinalization | 1:N |
| AcademicYear | GradeFinalization | 1:N |
| AcademicYear | FeeStructure | 1:N |
| GradeLevel | FeeStructure | 1:N |
| Enrollment | StudentCharge | 1:N |
| FeeStructure | StudentCharge | 1:N |
| Enrollment | Payment | 1:N |
| Enrollment | FinancialAdjustment | 1:N |
| Payment | FinancialAdjustment | 1:N |
| UserAccount | UserRole | 1:N |
| Role | UserRole | 1:N |
| Role | RolePermission | 1:N |
| Permission | RolePermission | 1:N |
| UserAccount | AuditLog | 1:N |

تُفرض العلاقات الإضافية التي تتضمن مراجع المستخدمين في سجلات الدرجات والمدفوعات والموافقات وعمليات الاعتماد باستخدام المفاتيح الخارجية (Foreign Keys).

## 6. المفاتيح وقيود سلامة البيانات (Key and Integrity Constraints)

### 6.1 المفاتيح الأساسية (Primary Keys)

يجب أن يمتلك كل كيان مفتاحًا أساسيًا يميز كل سجل بصورة فريدة.

تُستخدم المفاتيح الأساسية المركبة (Composite Primary Keys) في الجدولين:

- `UserRole`
- `RolePermission`

### 6.2 المفاتيح الخارجية (Foreign Keys)

يجب استخدام المفاتيح الخارجية لضمان صحة العلاقات بين السجلات المؤسسية.

كما يجب ألا تؤدي عمليات حذف السجلات المرجعية أو تعديلها إلى الإضرار بالمعلومات الأكاديمية والمالية التاريخية.

### 6.3 قيود عدم التكرار (Unique Constraints)

يجب تطبيق قيود عدم التكرار على المعرفات المؤسسية وأسماء المستخدمين والخصائص الأخرى التي تتطلب قيمًا فريدة.

ويجب تطبيق قيود التفرد المشروطة (Conditional Uniqueness) عندما تمنع قواعد العمل تكرار السجلات النشطة فقط.

### 6.4 قيود التحقق (Validation Constraints)

يجب أن تفرض قاعدة البيانات والتطبيق القيود المناسبة، ومنها:

- عدم السماح بمبالغ رسوم سالبة.
- اشتراط أن تكون مبالغ المدفوعات أكبر من الصفر.
- التحقق من وقوع الدرجات ضمن الحدود المسموح بها.
- التحقق من صحة نطاق تواريخ السنة الدراسية.
- إلزامية الحقول المطلوبة للعلاقات الأساسية.
- الالتزام بقيم الحالات المسموح بها للسجلات.

## 7. استراتيجية تطبيع قاعدة البيانات (Normalization Strategy)

يتبع التصميم المقترح مبادئ تطبيع قواعد البيانات العلائقية (Relational Database Normalization).

### الصيغة العادية الأولى (First Normal Form – 1NF)

يجب أن تحتوي الخصائص على قيم ذرية (Atomic Values)، مع تجنب مجموعات البيانات المتكررة داخل السجل.

### الصيغة العادية الثانية (Second Normal Form – 2NF)

يجب أن تعتمد الخصائص غير المفتاحية على المفتاح الأساسي بالكامل.

### الصيغة العادية الثالثة (Third Normal Form – 3NF)

يجب ألا تعتمد الخصائص غير المفتاحية اعتمادًا انتقاليًا (Transitive Dependency) على المفاتيح الأساسية من خلال خصائص غير مفتاحية أخرى.

يجب التحقق من التطبيع وفقًا للمخطط النهائي قبل التنفيذ الفعلي لقاعدة البيانات.

## 8. إدارة المعاملات (Transaction Management)

يجب أن تحافظ معاملات قاعدة البيانات على الاتساق بين العمليات المترابطة.

ومن أمثلة ذلك:

- تسجيل الطالب وإسناده إلى الشعبة الدراسية.
- اعتماد الدرجات وتحديث الحالات المرتبطة بها.
- تسجيل المدفوعات وتحديث الأرصدة المالية.
- تنفيذ التصحيحات المعتمدة على المعاملات المالية.

يجب أن توفر المعاملات خصائص:

**Atomicity, Consistency, Isolation, Durability (ACID)**

أي:

- **الذرية (Atomicity):** تنفيذ جميع أجزاء المعاملة أو التراجع عنها بالكامل.
- **الاتساق (Consistency):** المحافظة على قواعد سلامة البيانات قبل المعاملة وبعدها.
- **العزل (Isolation):** التحكم في تأثير المعاملات المتزامنة بعضها على بعض.
- **الديمومة (Durability):** استمرار حفظ التغييرات بعد اعتماد المعاملة بنجاح.

وذلك وفقًا للإمكانات التي يوفرها نظام إدارة قاعدة البيانات المختار.

## 9. أمن البيانات والاستعادة (Data Security and Recovery)

يجب أن يدعم تصميم قاعدة البيانات ما يلي:

- تقييد الوصول إلى قاعدة البيانات.
- التخزين الآمن للقيم المجزأة لكلمات المرور.
- التفويض المستند إلى الأدوار على مستوى التطبيق.
- تسجيل العمليات الحساسة في سجلات التدقيق.
- إنشاء نسخ احتياطية محمية من قاعدة البيانات.
- إجراءات استعادة منضبطة.
- المحافظة على التكامل المرجعي أثناء الاستعادة.

يجب أن تتوافق دورية النسخ الاحتياطي ومدد الاحتفاظ بالنسخ مع السياسات التشغيلية المعتمدة.

## 10. مخطط الكيانات والعلاقات (ERD Deliverable)

يجب أن يمثل مخطط الكيانات والعلاقات (Entity Relationship Diagram – ERD) الكيانات المقترحة والعلاقات بينها.

**مسار الملف المخطط إنشاؤه:**

`diagrams/ERD.png`

ويجب الاحتفاظ بنسخة مصدر قابلة للتحرير إلى جانب المخطط المصدّر.

كما يجب مراجعة المخطط ومطابقته مع تعريفات الكيانات النهائية وقيود العلاقات قبل البدء بالتنفيذ.

## 11. قرارات التصميم التي تتطلب تأكيدًا (Design Decisions Requiring Confirmation)

| معرف القرار | القرار المطلوب |
|---|---|
| DD-01 | اختيار نظام إدارة قاعدة البيانات العلائقية (DBMS). |
| DD-02 | تحديد هيكل التقييم الأكاديمي النهائي وفترات احتساب الدرجات. |
| DD-03 | تأكيد سياسات التعديلات المالية والمدفوعات الزائدة. |
| DD-04 | تحديد مدد الاحتفاظ بالسجلات المؤسسية. |
| DD-05 | تأكيد إسناد أدوار المستخدمين والتوزيع التفصيلي للصلاحيات. |

## 12. المستندات ذات الصلة (Related Documents)

| المستند | العلاقة |
|---|---|
| [SRS](../02-Requirements-Analysis/SRS.md) | يحدد متطلبات النظام ونطاقه. |
| [Functional Requirements](../02-Requirements-Analysis/Functional-Requirements.md) | يحدد العمليات المطلوبة المتعلقة بالبيانات. |
| [Business Rules](../03-System-Analysis/Business-Rules.md) | يحدد القيود المؤسسية التي تحكم البيانات. |
| [Process Analysis](../03-System-Analysis/Process-Analysis.md) | يحدد مخازن البيانات المنطقية وتدفقات المعلومات. |
| [System Architecture](System-Architecture.md) | يحدد معمارية التطبيق ونموذج نشر قاعدة البيانات. |
| [Interface Design](Interface-Design.md) | يحدد واجهات المستخدم التي تتفاعل مع قاعدة البيانات. |

## 13. الخاتمة (Conclusion)

يحدد تصميم قاعدة البيانات المقترح الكيانات العلائقية الرئيسية والعلاقات بينها وضوابط سلامة البيانات المطلوبة لنظام إدارة المدرسة.

ويدعم التصميم الإدارة المركزية للبيانات، واتساق السجلات المؤسسية، والوصول المنضبط لعدة مستخدمين، والمحافظة على المعلومات التاريخية.

كما يوفر الأساس اللازم لإعداد مخطط الكيانات والعلاقات (ERD) بصورة تفصيلية، وتنفيذ قاعدة البيانات فعليًا، وربطها بتطبيق سطح المكتب.

---

**نهاية المستند (End of Document)**
