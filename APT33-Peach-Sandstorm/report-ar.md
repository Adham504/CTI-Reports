# APT33 / Peach Sandstorm — تقرير استخبارات التهديدات

**التصنيف:** TLP:WHITE — قابل للمشاركة بحرية
**فترة الحملة:** فبراير – يوليو 2023
**الكاتب:** عدهم أيمن
**مستوى الثقة:** مرتفع
**رقم التقرير:** CTI-2023-IR-001-AR

---

## 1. الملخص التنفيذي

بين فبراير ويوليو 2023، شنّت مجموعة **APT33** المعروفة أيضاً بـ **Peach Sandstorm** وسابقاً بـ **Holmium** حملة واسعة النطاق تعتمد على هجمات **Password Spray (رش كلمات المرور)**، استهدفت آلاف المنظمات حول العالم.

ركّزت المجموعة على ثلاثة قطاعات استراتيجية: **الدفاع، والفضاء، والأدوية**، وكلها تخدم المصالح الوطنية الإيرانية بشكل مباشر في ظل العقوبات الدولية المشددة.

بعد الاختراق الأولي، استخدم المهاجمون مزيجاً من أدوات مفتوحة المصدر وأدوات مخصصة لاستطلاع الشبكات، وتثبيت الوجود، والتحرك الجانبي داخل البيئات المستهدفة، مع تسريب انتقائي للبيانات في حالات محدودة. لم تُستخدم أي أدوات تخريبية، مما يؤكد أن الدافع كان **Intelligence Collection (جمع المعلومات الاستخباراتية)** لا التخريب.

---

## 2. Diamond Model (نموذج الماسة)

### Adversary (المهاجم)

| Field | Detail |
|-------|--------|
| Name | APT33 / Peach Sandstorm |
| Aliases | Holmium / Elfin / Refined Kitten |
| Origin | Iran |
| Sponsor | الحكومة الإيرانية، مرتبط بالحرس الثوري IRGC |
| Type | Nation-state threat actor (جهة تهديد ترعاها دولة) |

> المصدر: Microsoft Threat Intelligence، سبتمبر 2023

---

### Capability (القدرات)

**Initial Access (الوصول الأولي):**

- هجمات **Password Spray** على نطاق واسع ضد آلاف المنظمات
- استغلال **CVE-2022-26134** — ثغرة RCE في Atlassian Confluence
- استغلال **CVE-2022-47966** — ثغرة RCE في Zoho ManageEngine

**Post-Compromise Tools (الأدوات المستخدمة بعد الاختراق):**

- `AzureHound` — استطلاع بيئة Azure السحابية
- `ROADtools` — الوصول إلى بيانات البيئة السحابية وتعدادها
- `Golang binary` — أداة استطلاع مخصصة بناها المهاجم
- `AnyDesk` — أداة Remote Access مُساء استخدامها لتثبيت الوجود Persistence
- `AD Explorer` — تعداد Active Directory
- `Azure Arc` — تثبيت Persistence عبر اشتراك Azure يتحكم فيه المهاجم
- Lateral Movement (تحرك جانبي) عبر بروتوكول **SMB**
- `Golden SAML` — تزوير بيانات اعتماد SAML للوصول السحابي

> المصدر: Microsoft Threat Intelligence، سبتمبر 2023 / Certfa Radar، سبتمبر 2023

**Attack Chain (سلسلة الهجوم):**

```
Password Spray
    ↓
Exploitation (استغلال الثغرات)
    ↓
Discovery (الاستطلاع)
    ↓
Persistence (تثبيت الوجود)
    ↓
Lateral Movement (التحرك الجانبي)
    ↓
Exfiltration (تسريب البيانات - حالات محدودة)
```

---

### Infrastructure (البنية التحتية)

> تحذير: عناوين IP مصدرها Certfa Radar. مستوى الثقة **منخفض-متوسط**. جميع القيم مُؤمَّنة (defanged). تحقق منها قبل الاستخدام العملياتي.

| Indicator | Type | Source | Confidence |
|-----------|------|--------|------------|
| `102[.]129.215.40` | IP Address | Certfa Radar | Low-Medium |
| `108[.]62.118.240` | IP Address | Certfa Radar | Low-Medium |
| `192[.]52.166.76` | IP Address | Certfa Radar | Low-Medium |
| `76[.]8.60.64` | IP Address | Certfa Radar | Low-Medium |

> المصدر: [Certfa Radar — Peach Sandstorm](https://radar.certfa.com/en/threats/view/3dd41ae0/)

---

### Victim (الضحية)

| Field | Detail |
|-------|--------|
| Sectors | Defense / Aerospace / Pharmaceuticals |
| Geography | الشرق الأوسط / الولايات المتحدة / أوروبا |
| Target Profile | منظمات استراتيجية عالية القيمة |
| Scale | آلاف المنظمات جرى استهدافها، متابعة انتقائية للاختراقات الناجحة |

> المصدر: Microsoft Threat Intelligence، سبتمبر 2023 / Certfa Radar، سبتمبر 2023

---

### Socio-Political Context (السياق الجيوسياسي)

- **Motive (الدافع):** Intelligence Collection لصالح الدولة الإيرانية
- **Intent (النية):** Espionage (تجسس)، لم تُنشر أي أدوات تخريبية
- **Strategic Driver (المحرك الاستراتيجي):** العزلة الدولية والعقوبات وانهيار المفاوضات النووية دفعت إيران إلى ملء فجوات المعلومات عبر العمليات السيبرانية

---

## 3. MITRE ATT&CK Techniques

| Tactic | Technique | ID | Tool / Method |
|--------|-----------|----|---------------|
| Initial Access | Password Spraying | T1110.003 | رش كلمات المرور ضد حسابات سحابية |
| Initial Access | Exploit Public-Facing Application | T1190 | CVE-2022-26134 / CVE-2022-47966 |
| Discovery | Cloud Service Discovery | T1526 | AzureHound / ROADtools |
| Discovery | Account Discovery | T1087 | AD Explorer |
| Persistence | Remote Access Software | T1219 | AnyDesk (مُساء استخدامه) |
| Persistence | Cloud Administration Command | T1098 | Azure Arc |
| Credential Access | Forge Web Credentials: SAML | T1606.002 | Golden SAML |
| Lateral Movement | Remote Services: SMB | T1021.002 | التحرك الجانبي عبر SMB |
| Exfiltration | Exfiltration Over C2 Channel | T1041 | حالات محدودة فقط |

> المصدر: Microsoft Threat Intelligence، سبتمبر 2023
> الملف الكامل: [MITRE ATT&CK — APT33 (G0064)](https://attack.mitre.org/groups/G0064/)

---

## 4. التحليل الجيوسياسي

### لماذا هذه القطاعات تحديداً؟

**قطاع الدفاع (Defense):**
اختراق المنظمات الدفاعية يمنح إيران إمكانية الوصول إلى تصاميم أنظمة الأسلحة والتكتيكات العسكرية وهياكل القيادة والخطط التشغيلية، وهي معلومات تعزز قدرتها العسكرية دون مواجهة مباشرة.

**قطاع الفضاء (Aerospace):**
يتداخل هذا القطاع بشكل مباشر مع تقنيات الصواريخ والاتصالات الفضائية. سرقة بياناته تساعد إيران في تطوير برنامجها للصواريخ الباليستية وفهم قدرات المراقبة لدى خصومها.

**قطاع الأدوية (Pharmaceuticals):**
قيّدت العقوبات الدولية قدرة إيران على استيراد الأدوية المتقدمة ومعدات التصنيع. سرقة Intellectual Property (الملكية الفكرية) الدوائية تتيح لها التصنيع محلياً، إذ لا تستطيع العقوبات منع ما تعلمته إيران صنعه بالفعل.

---

### لماذا هذا التوقيت؟ (فبراير – يوليو 2023)

مرّ اتفاق JCPOA النووي بمراحل حرجة بين عامَي 2022 و2023:

**2022:** فشلت مفاوضات إحياء الاتفاق بسبب الشروط الإيرانية المتشددة، واندلاع احتجاجات مهسا أميني داخلياً، وقرار إيران تزويد روسيا بطائرات مسيّرة عسكرية. كل ذلك عمّق عزلتها الدولية وأبعدها عن أي تسهيل في العقوبات.

**مطلع 2023:** تحوّل التركيز من اتفاق شامل إلى دبلوماسية خلفية غير رسمية تقتصر على تبادل الأسرى وإدارة التصعيد، لا على رفع العقوبات.

**النتيجة:** مع انسداد الأفق الدبلوماسي وتضييق العقوبات، لجأت إيران إلى **Cyber Espionage (التجسس السيبراني)** بديلاً عما لم تعد قادرة على الحصول عليه بالطرق الرسمية. توقيت الحملة ليس مصادفة بل يعكس قراراً استراتيجياً متعمداً.

---

### Espionage مقابل Sabotage — كيف تؤكد TTPs النية؟

| المؤشر | Espionage (تجسس) | Sabotage (تخريب) |
|--------|-----------------|-----------------|
| الهدف الأساسي | سرقة البيانات | تدمير الأنظمة وتعطيلها |
| الأدوات | AzureHound / ROADtools / AD Explorer | Wipers / Ransomware / برمجيات تدميرية |
| أسلوب Persistence | البقاء خفياً لأطول فترة ممكنة | غير ضروري غالباً |
| Exfiltration | نعم، انتقائي وموجّه | نادر |
| APT33 في هذه الحملة | مطابق تماماً | لا أدوات تدميرية |

**الخلاصة:** الغياب التام للأدوات التدميرية مع التسريب الانتقائي وأدوات Persistence طويلة الأمد يؤكد أن هذه الحملة كانت **Espionage (تجسساً) بحتاً**. إيران كانت بحاجة إلى معلومات لا إلى نفوذ.

---

## 5. IOC Reference (مؤشرات الاختراق)

| Type | Value | Confidence | Source |
|------|-------|------------|--------|
| IP | `102[.]129.215.40` | Low-Medium | Certfa Radar |
| IP | `108[.]62.118.240` | Low-Medium | Certfa Radar |
| IP | `192[.]52.166.76` | Low-Medium | Certfa Radar |
| IP | `76[.]8.60.64` | Low-Medium | Certfa Radar |
| CVE | CVE-2022-26134 | High | Microsoft / NIST NVD |
| CVE | CVE-2022-47966 | High | Microsoft / NIST NVD |
| Tool | AnyDesk | High | Microsoft Threat Intelligence |
| Tool | AzureHound | High | Microsoft Threat Intelligence |
| Tool | ROADtools | High | Microsoft Threat Intelligence |
| Tool | AD Explorer | High | Microsoft Threat Intelligence |

> تحذير: جميع عناوين IP مُؤمَّنة (defanged). تحقق من جميع المؤشرات مقابل قواعد بيانات التهديدات الحالية قبل الاستخدام العملياتي.

---

## 6. References (المراجع)

| # | Source | Link |
|---|--------|------|
| 1 | Microsoft Threat Intelligence — Peach Sandstorm، سبتمبر 2023 | [رابط](https://www.microsoft.com/en-us/security/blog/2023/09/14/peach-sandstorm-password-spray-campaigns-enable-intelligence-collection-at-high-value-targets/) |
| 2 | Certfa Radar — Peach Sandstorm، سبتمبر 2023 | [رابط](https://radar.certfa.com/en/threats/view/3dd41ae0/) |
| 3 | MITRE ATT&CK — APT33 (G0064) | [رابط](https://attack.mitre.org/groups/G0064/) |
| 4 | NIST NVD — CVE-2022-26134 | [رابط](https://nvd.nist.gov/vuln/detail/CVE-2022-26134) |
| 5 | NIST NVD — CVE-2022-47966 | [رابط](https://nvd.nist.gov/vuln/detail/CVE-2022-47966) |

---

*أُعدّ هذا التقرير لأغراض تعليمية وبناء المحفظة المهنية. TLP:WHITE — قابل للمشاركة بحرية.*
