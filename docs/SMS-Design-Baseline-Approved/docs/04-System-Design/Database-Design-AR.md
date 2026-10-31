# نظام إدارة المدرسة (School Management System)
## تصميم قاعدة البيانات — النسخة المنطقية النهائية (Database Design — Final Logical Baseline)

| بيانات المستند | القيمة |
|---|---|
| الإصدار | 2.0 |
| التاريخ | أكتوبر 2026 |
| الحالة | **القرارات المنطقية معتمدة؛ وقيود التنفيذ ما تزال قيد التحديد** |
| التطبيق | سطح مكتب فقط، لمدرسة أهلية واحدة |
| التخزين | قاعدة بيانات علائقية مركزية على الشبكة المحلية للمدرسة (LAN) |
| نظام إدارة قاعدة البيانات (DBMS) | لم يُحدّد بعد |

> **حدود الاعتماد النهائي:** يمثل هذا المستند النسخة المرجعية النهائية **للتصميم المنطقي** بعد إدراج قرارات المراجعة الأربعة المعتمدة. ولا يعني ذلك اعتماد مخطط تنفيذي لنظام إدارة قواعد بيانات محدد، أو ملفات ترحيل SQL، أو السياسات المدرسية التي لم يُبت فيها.

## 1. الغرض والنطاق (Purpose and Scope)

يحدد المستند الكيانات وخصائصها، وأنواع العلاقات وتعددها (Cardinalities)، وقواعد العمل الثابتة التي يجب ألا تُنتهك (Business Invariants)، وحدود المعاملات (Transaction Boundaries) في نظام إدارة المدرسة (SMS). ويخدم سبع وحدات: إدارة الطلاب، والمعلمين، والصفوف، والدرجات، والرسوم، والمستخدمين، والتقارير. تستمد التقارير بياناتها من الجداول التشغيلية، ولا تحتاج إلى كيان تشغيلي مستقل. وتمثل سجلات التدقيق مجالًا مشتركًا بين الوحدات. لا يشمل النطاق الحضور، أو الجداول الزمنية، أو إدارة الاختبارات كوحدة مستقلة، أو بوابات الويب والهواتف، أو إدارة مدارس متعددة.

## 2. الافتراضات المعمارية (Architectural Assumptions)

- يحتوي تطبيق سطح المكتب على طبقات العرض ومنطق الأعمال والوصول إلى البيانات؛ وتتصل أجهزة المستخدمين بقاعدة بيانات علائقية واحدة تُدار مركزيًا عبر شبكة المدرسة المحلية.
- تضم كل سنة دراسية فترات دراسية قابلة للتهيئة؛ وتبقى سلالم الدرجات وأوزان التقييم وصلاحيات الاعتماد قرارات مؤسسية قابلة للضبط.
- تُخزَّن المبالغ المالية بحساب عشري دقيق؛ ويجب تحديد العملة وقواعد التقريب وسياسات الرسوم قبل التنفيذ الفعلي.
- تُحفظ الأحداث الأكاديمية والمالية التاريخية، ولا تُستبدل قيمها بصمت.
- تتطلب كل عملية محمية مستخدمًا موثق الهوية وتفويضًا قائمًا على الأدوار (RBAC).

## 3. قائمة الكيانات (Entity Inventory)

| المجال | الكيانات | العدد |
|---|---|---:|
| الطلاب | `Student`, `Enrollment`, `SectionAssignment` | 3 |
| المعلمون | `Teacher`, `TeachingAssignment` | 2 |
| الهيكل الأكاديمي | `AcademicYear`, `AcademicTerm`, `GradeLevel`, `ClassSection`, `Subject`, `SectionSubject` | 6 |
| الدرجات | `AssessmentComponent`, `StudentGrade`, `GradeFinalization` | 3 |
| الرسوم | `FeeStructure`, `StudentCharge`, `Payment`, `PaymentAllocation`, `FinancialAdjustment` | 5 |
| الوصول والصلاحيات | `UserAccount`, `Role`, `Permission`, `UserRole`, `RolePermission` | 5 |
| التدقيق | `AuditLog` | 1 |
| **الإجمالي** | | **25** |

## 4. قاموس الكيانات المنطقي (Logical Entity Dictionary)

**الرموز:** `PK` مفتاح أساسي (Primary Key)؛ و`FK → X` مفتاح خارجي يشير إلى الكيان X؛ و`UQ` قيد تفرد (Unique)؛ و`FK?` مفتاح خارجي اختياري يسمح بالقيمة الفارغة (Nullable). تُعد الحقول غير المعلَّمة بأنها اختيارية إلزامية، ما لم تنص قاعدة مرتبطة بحالة السجل أو سياسة معتمدة على غير ذلك. تستخدم جميع الكيانات معرفات بديلة (Surrogate Keys)، باستثناء الجداول الوسيطة لنظام الصلاحيات التي تستخدم مفاتيح أساسية مركبة. تُحدد أنواع البيانات الفعلية بعد اختيار DBMS.

**ملاحظة:** تُترك أسماء الجداول والأعمدة بالإنجليزية كما هي في المخطط، لأنها معرفات تقنية وليست نصوصًا وصفية.

### 4.1 إدارة الطلاب (Student Management)

#### الطالب (`Student`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `StudentID` | PK |
| `StudentNumber` | UQ |
| `FirstName` | — |
| `LastName` | — |
| `DateOfBirth` | — |
| `Gender` | — |
| `ContactInformation` | — |
| `StudentStatus` | — |
| `CreatedAt` | — |

#### القيد الدراسي (`Enrollment`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `EnrollmentID` | PK |
| `StudentID` | FK → `Student` |
| `AcademicYearID` | FK → `AcademicYear` |
| `EnrollmentDate` | — |
| `EnrollmentStatus` | — |

#### إسناد الطالب إلى الشعبة (`SectionAssignment`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `SectionAssignmentID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `ClassSectionID` | FK → `ClassSection` |
| `EffectiveFrom` | — |
| `EffectiveTo` | — |

### 4.2 إدارة المعلمين (Teacher Management)

#### المعلم (`Teacher`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `TeacherID` | PK |
| `TeacherNumber` | UQ |
| `FirstName` | — |
| `LastName` | — |
| `ContactInformation` | — |
| `TeacherStatus` | — |
| `CreatedAt` | — |

#### التكليف التدريسي (`TeachingAssignment`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `AssignmentID` | PK |
| `TeacherID` | FK → `Teacher` |
| `SectionSubjectID` | FK → `SectionSubject` |
| `AssignmentStatus` | — |

### 4.3 الهيكل الأكاديمي (Academic Structure)

#### السنة الدراسية (`AcademicYear`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `AcademicYearID` | PK |
| `YearName` | UQ |
| `StartDate` | — |
| `EndDate` | — |
| `YearStatus` | — |

#### الفترة الدراسية (`AcademicTerm`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `TermID` | PK |
| `AcademicYearID` | FK → `AcademicYear` |
| `TermName` | — |
| `StartDate` | — |
| `EndDate` | — |
| `TermStatus` | — |

#### الصف الدراسي (`GradeLevel`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `GradeLevelID` | PK |
| `GradeName` | UQ |
| `GradeOrder` | — |

#### الشعبة الدراسية (`ClassSection`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `ClassSectionID` | PK |
| `AcademicYearID` | FK → `AcademicYear` |
| `GradeLevelID` | FK → `GradeLevel` |
| `SectionName` | — |
| `SectionStatus` | — |

#### المادة الدراسية (`Subject`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `SubjectID` | PK |
| `SubjectCode` | UQ |
| `SubjectName` | — |
| `SubjectStatus` | — |

#### مادة الشعبة (`SectionSubject`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `SectionSubjectID` | PK |
| `ClassSectionID` | FK → `ClassSection` |
| `SubjectID` | FK → `Subject` |
| `SectionSubjectStatus` | — |

### 4.4 إدارة الدرجات (Grade Management)

#### مكوّن التقييم (`AssessmentComponent`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `AssessmentComponentID` | PK |
| `SectionSubjectID` | FK → `SectionSubject` |
| `TermID` | FK → `AcademicTerm` |
| `ComponentName` | — |
| `MaximumScore` | — |
| `Weight` | — |
| `AssessmentDate` | — |

#### درجة الطالب (`StudentGrade`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `StudentGradeID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `SectionAssignmentID` | FK → `SectionAssignment` |
| `AssessmentComponentID` | FK → `AssessmentComponent` |
| `Score` | — |
| `GradeStatus` | — |
| `RecordedBy` | FK → `UserAccount` |
| `RecordedAt` | — |

#### اعتماد الدرجات (`GradeFinalization`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `FinalizationID` | PK |
| `SectionSubjectID` | FK → `SectionSubject` |
| `TermID` | FK → `AcademicTerm` |
| `FinalizedBy` | FK → `UserAccount` |
| `FinalizedAt` | — |
| `FinalizationStatus` | — |

### 4.5 إدارة الرسوم (Fee Management)

#### هيكل الرسوم (`FeeStructure`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `FeeStructureID` | PK |
| `AcademicYearID` | FK → `AcademicYear` |
| `GradeLevelID` | FK → `GradeLevel` |
| `FeeName` | — |
| `FeeAmount` | — |
| `FeeStatus` | — |

#### الرسوم المستحقة على الطالب (`StudentCharge`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `ChargeID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `FeeStructureID` | FK → `FeeStructure` |
| `ChargeAmount` | — |
| `ChargeDate` | — |
| `ChargeStatus` | — |

#### المدفوعات (`Payment`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `PaymentID` | PK |
| `PaymentNumber` | UQ |
| `EnrollmentID` | FK → `Enrollment` |
| `PaymentAmount` | — |
| `PaymentDate` | — |
| `PaymentStatus` | — |
| `RecordedBy` | FK → `UserAccount` |
| `RecordedAt` | — |

#### توزيع المدفوعات (`PaymentAllocation`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `AllocationID` | PK |
| `PaymentID` | FK → `Payment` |
| `ChargeID` | FK → `StudentCharge` |
| `AllocatedAmount` | — |
| `AllocatedAt` | — |

#### التعديل المالي (`FinancialAdjustment`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `AdjustmentID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `PaymentID` | FK? → `Payment` |
| `ChargeID` | FK? → `StudentCharge` |
| `AdjustmentType` | — |
| `AdjustmentAmount` | — |
| `AdjustmentReason` | — |
| `ApprovedBy` | FK → `UserAccount` |
| `CreatedAt` | — |

### 4.6 إدارة المستخدمين (User Management)

#### حساب المستخدم (`UserAccount`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `UserID` | PK |
| `Username` | UQ |
| `PasswordHash` | — |
| `AccountStatus` | — |
| `CreatedAt` | — |

#### الدور (`Role`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `RoleID` | PK |
| `RoleName` | UQ |
| `RoleDescription` | — |

#### الصلاحية (`Permission`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `PermissionID` | PK |
| `PermissionCode` | UQ |
| `PermissionDescription` | — |

#### أدوار المستخدم (`UserRole`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `UserID` | PK/FK → `UserAccount` |
| `RoleID` | PK/FK → `Role` |

#### صلاحيات الدور (`RolePermission`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `RoleID` | PK/FK → `Role` |
| `PermissionID` | PK/FK → `Permission` |

### 4.7 التدقيق (Audit)

#### سجل التدقيق (`AuditLog`)

| الخاصية (Attribute) | القيد أو المرجع (Constraint / Reference) |
|---|---|
| `AuditID` | PK |
| `UserID` | FK? → `UserAccount` |
| `ActionType` | — |
| `EntityName` | — |
| `RecordIdentifier` | — |
| `OldValue` | — |
| `NewValue` | — |
| `EventTimestamp` | — |

## 5. العلاقات وتعددها (Relationships and Cardinality)

كل علاقة أدناه هي **واحد إلى متعدد (1:N)** من الكيان الأساسي المشار إليه إلى الكيان التابع الذي يحتوي المفتاح الخارجي. يضمن المفتاح الخارجي وجود السجل المرجعي، بينما تُفرض شروط الأهلية المتعلقة بسجلات متعددة أو بالفترات الزمنية وفق القسم السادس.

| الكيان الأساسي (1) | الكيان التابع (N) | المفتاح الخارجي في الكيان التابع |
|---|---|---|
| `Student` | `Enrollment` | `StudentID` |
| `AcademicYear` | `Enrollment` | `AcademicYearID` |
| `Enrollment` | `SectionAssignment` | `EnrollmentID` |
| `ClassSection` | `SectionAssignment` | `ClassSectionID` |
| `Teacher` | `TeachingAssignment` | `TeacherID` |
| `SectionSubject` | `TeachingAssignment` | `SectionSubjectID` |
| `AcademicYear` | `AcademicTerm` | `AcademicYearID` |
| `AcademicYear` | `ClassSection` | `AcademicYearID` |
| `GradeLevel` | `ClassSection` | `GradeLevelID` |
| `ClassSection` | `SectionSubject` | `ClassSectionID` |
| `Subject` | `SectionSubject` | `SubjectID` |
| `SectionSubject` | `AssessmentComponent` | `SectionSubjectID` |
| `AcademicTerm` | `AssessmentComponent` | `TermID` |
| `Enrollment` | `StudentGrade` | `EnrollmentID` |
| `SectionAssignment` | `StudentGrade` | `SectionAssignmentID` |
| `AssessmentComponent` | `StudentGrade` | `AssessmentComponentID` |
| `UserAccount` | `StudentGrade` | `RecordedBy` |
| `SectionSubject` | `GradeFinalization` | `SectionSubjectID` |
| `AcademicTerm` | `GradeFinalization` | `TermID` |
| `UserAccount` | `GradeFinalization` | `FinalizedBy` |
| `AcademicYear` | `FeeStructure` | `AcademicYearID` |
| `GradeLevel` | `FeeStructure` | `GradeLevelID` |
| `Enrollment` | `StudentCharge` | `EnrollmentID` |
| `FeeStructure` | `StudentCharge` | `FeeStructureID` |
| `Enrollment` | `Payment` | `EnrollmentID` |
| `UserAccount` | `Payment` | `RecordedBy` |
| `Payment` | `PaymentAllocation` | `PaymentID` |
| `StudentCharge` | `PaymentAllocation` | `ChargeID` |
| `Enrollment` | `FinancialAdjustment` | `EnrollmentID` |
| `Payment` | `FinancialAdjustment` | `PaymentID (optional)` |
| `StudentCharge` | `FinancialAdjustment` | `ChargeID (optional)` |
| `UserAccount` | `FinancialAdjustment` | `ApprovedBy` |
| `UserAccount` | `UserRole` | `UserID` |
| `Role` | `UserRole` | `RoleID` |
| `Role` | `RolePermission` | `RoleID` |
| `Permission` | `RolePermission` | `PermissionID` |
| `UserAccount` | `AuditLog` | `UserID (optional)` |

**العلاقات متعدد إلى متعدد (Many-to-Many):** `UserAccount ↔ Role` بواسطة `UserRole`؛ و`Role ↔ Permission` بواسطة `RolePermission`؛ و`ClassSection ↔ Subject` بواسطة `SectionSubject`؛ و`Payment ↔ StudentCharge` بواسطة `PaymentAllocation`.

## 6. قواعد العمل الثابتة وسلامة البيانات (Business Invariants and Integrity Rules)

### 6.1 القيد الدراسي وتاريخ الشعب (Enrollment and Section History)

- **ENR-01:** يُسمح بوجود **قيد دراسي نشط واحد على الأكثر** لكل زوج `(StudentID, AcademicYearID)`. ويجوز الاحتفاظ بالقيود السابقة المغلقة وفق السياسة المعتمدة.
- **ENR-02:** فترات `SectionAssignment` نصف مفتوحة: `[EffectiveFrom, EffectiveTo)`؛ أي تشمل تاريخ البداية ولا تشمل تاريخ النهاية. وتشير `EffectiveTo = NULL` إلى إسناد مفتوح النهاية. يُمنع تداخل فترات الإسناد للقيد الدراسي نفسه.
- **ENR-03:** يجب أن تساوي السنة الدراسية للشعبة `ClassSection.AcademicYearID` السنة الدراسية للقيد المرتبط `Enrollment.AcademicYearID`.
- **ENR-04:** لا يجوز وجود أكثر من إسناد حالي مفتوح للشعبة لكل قيد. تُغلق الفترة السابقة وتُفتح التالية في معاملة واحدة عند النقل، مع الاحتفاظ بالسجلات التاريخية.
- **ENR-05:** يجب أن يحافظ نقل الطالب على أهليته للصف الدراسي وفق سياسة المدرسة؛ ولا يجوز تغيير الصف الدراسي بمجرد تغيير الشعبة.

### 6.2 الهيكل الأكاديمي والدرجات (Academic Structure and Grading)

- **GRD-01:** يجب أن تقع تواريخ `AcademicTerm` ضمن السنة الدراسية `AcademicYear` المرتبطة بها، وأن يكون اسم الفترة فريدًا داخل السنة. ويبقى السماح بتداخل الفترات قرارًا مؤسسيًا يحتاج إلى تأكيد.
- **GRD-02:** الزوج `(ClassSectionID, SubjectID)` فريد في `SectionSubject`؛ وتُستمد السنة الدراسية لمادة الشعبة من الشعبة نفسها.
- **GRD-03:** يجب أن ينتمي كل من `AssessmentComponent.TermID` و`SectionSubjectID` إلى السنة الدراسية نفسها. ويجب أن يقع `AssessmentDate` ضمن الفترة المحددة، مع مراعاة سياسة المدرسة.
- **GRD-04:** يجب أن تكون `MaximumScore > 0` و`Weight >= 0`، وأن تحقق كل درجة `StudentGrade.Score` الشرط `0 <= Score <= MaximumScore`. وتبقى مجاميع الأوزان وصيغ الحساب قابلة للتهيئة.
- **GRD-05:** يجب أن يشير `StudentGrade.SectionAssignmentID` إلى إسناد شعبة تابع للقيد الدراسي `EnrollmentID` نفسه، وأن تطابق شعبة ذلك الإسناد شعبة `AssessmentComponent.SectionSubject.ClassSectionID`. ويجب أن يكون الإسناد ساريًا في تاريخ `AssessmentDate`. وبذلك تبقى أهلية الطالب التاريخية صحيحة بعد انتقاله.
- **GRD-06:** يُسمح بنتيجة واحدة فريدة لكل `(EnrollmentID, AssessmentComponentID)`، ما لم تُعتمد لاحقًا سياسة منفصلة لإعادة الاختبار أو إصدار نسخ من النتائج.
- **GRD-07:** يجب أن يمتلك المستخدم المسجّل للدرجة صلاحية إدخال الدرجات وتكليفًا تدريسيًا صالحًا لمادة الشعبة `SectionSubject`، أو صلاحية استثناء معتمدة صراحةً.
- **GRD-08:** لا يجوز وجود أكثر من سجل اعتماد حالي واحد لكل `(SectionSubjectID, TermID)`؛ ويتطلب الاعتماد أو إعادة فتح الدرجات إجراءً مصرحًا به وسجل تدقيق. ولا تُعدَّل النتائج المعتمدة بصمت.

### 6.3 الرسوم والمدفوعات والتصحيحات (Fees, Payments, and Corrections)

- **FIN-01:** يجب أن تكون `FeeStructure.FeeAmount >= 0` و`StudentCharge.ChargeAmount >= 0` و`Payment.PaymentAmount > 0` و`PaymentAllocation.AllocatedAmount > 0`.
- **FIN-02:** يجب أن يستند `StudentCharge` إلى هيكل رسوم من السنة الدراسية للقيد، والصف الدراسي المنطبق على الطالب في تاريخ فرض الرسم.
- **FIN-03:** يجب أن تتبع كل دفعة وجميع توزيعاتها **القيد الدراسي نفسه**.
- **FIN-04:** لكل دفعة **مرحّلة/مثبتة (Posted)**، يجب أن يتحقق `SUM(PaymentAllocation.AllocatedAmount) = Payment.PaymentAmount`. يُفرض هذا الشرط المتعلق بعدة صفوف عند اعتماد المعاملة، وليس بقيد `CHECK` على صف واحد.
- **FIN-05:** لا يجوز أن يتجاوز مجموع التوزيعات الفعالة على رسم ما قيمته المستحقة الفعلية. يرفض النظام أي دفعة تتجاوز إجمالي الرصيد المتبقي على الطالب؛ **ولا يدعم الأرصدة الدائنة غير المخصصة أو الدفع الزائد**.
- **FIN-06:** تُعتمد الدفعة الجديدة وتوزيعاتها وأي آثار على الأرصدة معًا في معاملة ذرية واحدة، مع ضبط التزامن على الرسوم المتأثرة.
- **FIN-07:** تصبح مبالغ الدفعات والتوزيعات الأصلية غير قابلة للتعديل بعد ترحيلها. تُوثق التصحيحات في `FinancialAdjustment` ومن خلال إجراء عكس/إبطال معتمد. تبقى الدفعة الملغاة وتوزيعاتها الأصلية محفوظة دليلًا تاريخيًا، لكنها تُستبعد من حساب المدفوعات الفعالة.
- **FIN-08:** تميز `FinancialAdjustment.AdjustmentType` بين `PAYMENT_VOID` (إبطال دفعة) و`CHARGE_ADJUSTMENT` (تعديل رسم مستحق). يجب أن يشير إبطال الدفعة إلى `PaymentID`، وأن يشير تعديل الرسم إلى `ChargeID`. ويجب أن يطابق القيد الدراسي السجل المرجعي، وأن يتضمن التعديل موافقًا وسببًا وتوقيتًا.
- **FIN-09:** لا يجوز أن تخفض تعديلات الرسوم القيمة المستحقة الفعلية إلى أقل من المبلغ المسدد بتوزيعات فعالة؛ ويجب عند الحاجة عكس الدفعة وإعادة توزيعها أولًا.
- **FIN-10:** تُحتسب الأرصدة المتبقية **اشتقاقيًا** من الرسوم والتعديلات الفعالة مطروحًا منها التوزيعات الفعالة للدفعات المرحلة؛ ولا يُخزن حقل رصيد مستقل قابل للتعديل اليدوي.

### 6.4 الهوية والتدقيق ودورة حياة السجلات (Identity, Auditing, and Lifecycle)

- **SEC-01:** يجب أن تكون أسماء المستخدمين والأدوار ورموز الصلاحيات والأرقام المؤسسية فريدة؛ وتُخزن كلمات المرور فقط بوصفها قيم تجزئة آمنة مع ملح (Salted Password Hashes) باستخدام خوارزمية مناسبة لتجزئة كلمات المرور.
- **SEC-02:** يستخدم `UserRole` و`RolePermission` مفاتيح أساسية مركبة؛ وتُفحص الصلاحيات الفعلية في طبقة منطق الأعمال عند كل عملية محمية.
- **SEC-03:** تُسجل التغييرات والموافقات الحساسة في `AuditLog` مع المنفذ والعملية والهدف والتوقيت؛ ويجب حماية السجلات من التعديل بعد إضافتها. لا يُسمح بغياب معرف المنفذ إلا للأحداث الصادرة عن النظام والمُوثقة بوضوح.
- **SEC-04:** لا يجوز حذف السجلات الأكاديمية والمالية وسجلات التدقيق التاريخية حذفًا نهائيًا عبر إجراءات التطبيق المعتادة.

## 7. التحقق وحدود المعاملات (Validation and Transaction Boundaries)

| العملية | الإجراءات التي يجب تنفيذها ذريًا | حماية التزامن وسلامة البيانات |
|---|---|---|
| نقل طالب | إغلاق إسناد الشعبة السابق؛ وإنشاء الإسناد الجديد؛ وتسجيل التدقيق | منع تداخل الفترات وإسناد شعبة من سنة دراسية مختلفة |
| إدخال درجة | التحقق من الإسناد والتاريخ وصلاحية المعلم وحدود الدرجة؛ وحفظ الدرجة والتدقيق | نتيجة فريدة وإسناد شعبة تاريخي صحيح |
| اعتماد الدرجات | التحقق من النتائج المؤهلة؛ وإنشاء/تحديث حالة الاعتماد؛ وتسجيل التدقيق | ضبط التزامن على نطاق الدرجات ومنع التعديل اللاحق غير المصرح به |
| ترحيل دفعة | التحقق من الأرصدة؛ وإنشاء الدفعة وتوزيعاتها؛ وتسجيل التدقيق | قفل الرسوم أو القيد المتأثر ومنع الدفع الزائد والتوزيع المكرر |
| تصحيح دفعة | اعتماد التعديل وتسجيله؛ وإبطال الدفعة الأصلية عند الحاجة؛ وتسجيل التدقيق | عدم تعديل المبالغ الأصلية وإعادة احتساب التوزيعات الفعالة باتساق |
| تعديل رسم مستحق | التحقق من الموافقة وحدود المبلغ المسدد؛ وتسجيل التعديل والتدقيق | منع نشوء رصيد مستحق فعلي سالب |

**نموذج الفرض (Enforcement Model):** تُستخدم قيود قاعدة البيانات `PK` و`FK` و`UNIQUE` و`NOT NULL` و`CHECK` حيثما أمكن؛ وتُستخدم معاملات منطق الأعمال للشروط التي تشمل عدة جداول أو فترات زمنية؛ ثم تُضاف فهارس جزئية (Partial Indexes) أو قيود منع التداخل (Exclusion Constraints) أو مشغلات (Triggers) أو آليات قفل وفق إمكانات DBMS المختار. لا يكفي التحقق في التطبيق وحده للحماية من الكتابات المتزامنة.

## 8. التطبيع وأنواع البيانات (Normalization and Data Types)

يستهدف التصميم المنطقي الصيغة العادية الثالثة (3NF): تحتوي كل خاصية على قيمة ذرية، وتعتمد كل خاصية غير مفتاحية على مفتاحها الكامل، وتُفصل الحقائق المتكررة والعلاقات متعدد إلى متعدد في جداول وسيطة. يجب التحقق من التطبيع أثناء التصميم الفعلي، ولا سيما `ContactInformation` (تُفصل إلى سجلات اتصال محددة النوع إذا لزم تخزين عدة وسائل اتصال)، و`OldValue`/`NewValue` في التدقيق (لقطات بيانات منظمة، وليست حقائق تشغيلية علائقية).

تُستخدم أنواع التاريخ والطابع الزمني الأصلية في DBMS، والأنواع العشرية الدقيقة للمبالغ والدرجات، ومعرفات ذات أطوال مناسبة، وقيود حالات محددة، وفهارس على المفاتيح الخارجية ومرشحات الاستعلام الشائعة مثل `(AcademicYearID, ClassSectionID, TermID)` حسب الحاجة. يُمنع استخدام أعداد الفاصلة العائمة (Floating-Point) لتخزين الأموال.

## 9. الأمان والنسخ الاحتياطي والاستعادة (Security, Backup, and Recovery)

يجب تطبيق مبدأ أقل الصلاحيات على حسابات قاعدة البيانات، وحفظ الأسرار بصورة آمنة، وتشفير اتصال التطبيق بقاعدة البيانات عند دعم ذلك، والتحقق من RBAC، وحماية سجلات التدقيق، وجدولة النسخ الاحتياطية، واختبار إجراءات الاستعادة. يجب أن تعتمد المدرسة دورية النسخ ومدد الاحتفاظ وأهداف التعافي.

## 10. ملفات ERD (ERD Deliverables)

- [مخطط ERD الكامل (PNG)](diagrams/ERD.png) — صورة كاملة الدقة.
- [مخطط ERD الكامل (SVG)](diagrams/ERD.svg) — صورة قابلة للتكبير دون فقدان الدقة.
- [مخطط ERD قابل للتحرير (draw.io)](diagrams/ERD.drawio) — كيانات وروابط قابلة للتعديل.
- [المصدر البرمجي Graphviz](diagrams/ERD.dot) — لإعادة توليد المخطط.
- مخططات تفصيلية: [الطلاب](diagrams/ERD-Students.png)، [الهيكل الأكاديمي](diagrams/ERD-Academic.png)، [الدرجات](diagrams/ERD-Grades.png)، [المالية](diagrams/ERD-Finance.png)، [المستخدمون والصلاحيات](diagrams/ERD-Access.png).

يضم المخطط **25 كيانًا و37 علاقة مفاتيح خارجية صريحة**. وهو يمثل **العلاقات الهيكلية**؛ أما قواعد العمل التي تشمل جداول متعددة أو فترات زمنية فموضحة في القسم السادس ولا يمكن التعبير عنها بالكامل بروابط ERD العادية.

## 11. القرارات المعتمدة (Approved Decisions)

| المعرف | القرار | الحالة |
|---|---|---|
| DD-01 | فصل `Enrollment` عن السجل التاريخي لإسنادات `SectionAssignment` | معتمد |
| DD-02 | إضافة `AcademicTerm` و`SectionSubject` | معتمد |
| DD-03 | إبقاء مكونات التقييم وأوزانها قابلة للتهيئة | معتمد |
| DD-04 | إضافة `PaymentAllocation` لتوزيع المدفوعات على الرسوم | معتمد |
| DD-05 | رفض المدفوعات التي تتجاوز الرصيد المستحق | معتمد |
| DD-06 | الحفاظ على المعاملات الأصلية وتوثيق التصحيحات المالية | معتمد |
| DD-07 | تطبيق قيود قاعدة البيانات وقواعد الأعمال والمعاملات الذرية الآمنة ضد التعارض | معتمد |

## 12. قرارات التنفيذ التي لم تُحسم (Implementation Decisions Still Open)

| المعرف | القرار المطلوب |
|---|---|
| PD-01 | اختيار DBMS وبرنامج الاتصال أو ORM والقيود الخاصة به |
| PD-02 | تحديد سلالم الدرجات ومجموع الأوزان وسياسات الفترات وصلاحية الاعتماد |
| PD-03 | تحديد خطوات التفويض والتسوية التفصيلية لعكس المعاملات المالية |
| PD-04 | تحديد العملة والدقة العشرية والتقريب وسياسات الاحتفاظ والنسخ الاحتياطي |
| PD-05 | اعتماد مصفوفة الصلاحيات النهائية RBAC، بما فيها صلاحيات الاستثناء الإداري |
| PD-06 | تحديد الحاجة إلى فصل وسائل اتصال الطلاب في سجلات مستقلة مطبّعة |

## 13. المستندات ذات الصلة (Related Documents)

| المستند | العلاقة |
|---|---|
| [SRS](../02-Requirements-Analysis/SRS.md) | متطلبات النظام ونطاقه |
| [Functional Requirements](../02-Requirements-Analysis/Functional-Requirements.md) | المتطلبات الوظيفية |
| [Non-Functional Requirements](../02-Requirements-Analysis/Non-Functional-Requirements.md) | قيود الأمان والموثوقية والجودة |
| [Business Rules](../03-System-Analysis/Business-Rules.md) | القواعد المؤسسية |
| [Process Analysis](../03-System-Analysis/Process-Analysis.md) | العمليات التشغيلية وتدفقات البيانات |
| [System Architecture](System-Architecture.md) | معمارية سطح المكتب ونشر قاعدة البيانات المركزية |
| [Interface Design](Interface-Design.md) | تصميم واجهات سطح المكتب لاحقًا |

## 14. الخاتمة (Conclusion)

تحدد النسخة المرجعية النهائية للتصميم المنطقي **25 كيانًا** و**37 علاقة هيكلية بمفاتيح خارجية** لدعم نطاق نظام إدارة المدرسة المعتمد. وقد أُدرجت صراحةً سجلات انتقال الطلاب بين الشعب، والدرجات المرتبطة بالفترات الدراسية، وتوزيع المدفوعات على الرسوم، والرفض الصارم للدفع الزائد، وضوابط سلامة البيانات متعددة المستويات. ويبقى تنفيذ SQL الفعلي والسياسات المؤسسية غير المحسومة خطوات اعتماد مستقلة.

---
**نهاية المستند (End of Document)**
