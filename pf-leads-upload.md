# رفع ليدز بروبرتي فايندر على The Arabians (كل يوم)

أحمد بيبعت كل يوم ملف CSV فيه ليدز بروبرتي فايندر الجديدة. المطلوب: ندخلها في السيستم (The Arabians CRM) ونوزعها على السيلز.
القواعد دي حطها أحمد يوم 2026-11-07. اتكلم معاه بالعامية المصرية.

## الأماكن
- الشيت: «The Arabians»، id `1fG3q6R5qSCZYgG18pImaFr58UH09qDIcTrSGHJX6nJo`، تاب `Leads` (34 عمود).
- مين معاه الليد **دلوقتي**: تاب `LeadState` (id، holderId، holder، stage، why). عمود owner في Leads مش كفاية، لأن السيستم بينقل الليدز. stage: s1، claimed، due، active، pool (يعني Open to all)، closed.
- السيلز ومين أونلاين: تاب `Team` (id، name، …، lastSeen بالـ ms). أونلاين = فتح اللينك في آخر 3 ساعات.
- مشروع كل إعلان: شيت «Daily Ads»، id `1eMUZghJ7jIPQOjsC49xl0-IvStTF13fLkJ1gEQadY_o`، تابات `Prem Nov` و`Stand Nov` (عمود Ref = الريفرنس، وProject، وType، وsales name). في الشهور الجاية اسم التاب بيتغير (Prem Dec …).
- قواعد السيستم كلها: `RULES.md` و`HANDOFF.md` في فولدر الدرايف «The Arabians CRM — project files». ممنوع تعديل الكود من الشغلانة دي.

## الملف اللي بيوصل
CSV متصدّر من بروبرتي فايندر. الأعمدة المهمة: `channel`، `sender_name`، `sender_phone`، `sender_email`، `whatsapp_username`، `agent_name` (السيلز صاحب الإعلان)، `listing_reference` (الريفرنس)، `created_at` (UTC، ويتحوّل للقاهرة بزيادة ساعتين في الشتا و3 في الصيف).
الملف بيبقى فيه ليدز قديمة كتير، فخد بس الفترة اللي أحمد بيقولها، بتوقيت القاهرة.

أسماء السيلز في بروبرتي فايندر وما يقابلها في Team:

| بروبرتي فايندر | Team | id |
|---|---|---|
| Ahmed Sese | Ahmed Sisi | `00mli3fe5cp6grro9f2i` |
| Adham Mohamed | Adham | `vcfenmkmjrg1yjhe7mn5` |
| Abdelhamid Khafagy | Abdelhamid | `mu3ikdf4jr822boduru6` |
| Ammar Ahmed | Ammar | `2ru2rg0l1jbkgkqlc2rf` |
| Mahmoud Essam | Mahmoud Essam | `h3mpsazqi2z6fqfwdmls` |
| Amr Essam | Amr Essam | `1d56peaxp0ty1vfneu8i` |
| (مش موجودين في الملف لحد دلوقتي) | Ahmed Sherif / Mahmoud Eid | `hqt3ew3w29qchznkoco0` / `5xu2zfb30dubmoohtz0o` |

لو ظهر اسم جديد مش في الجدول، اسأل أحمد قبل ما ترفع ليدزه.

## قواعد التوزيع
1. **كل ليد يروح للسيلز صاحب الإعلان** اللي العميل جه عليه (`agent_name`).
2. **الليد اللي من غير رقم موبايل ما يترفعش** (زي ليد واتساب جاي بيوزرنيم بس). بس اذكره في التقرير.
3. **الليد اللي من غير سيلز ولا ريفرنس** (زي اللي جاي بالإيميل) يروح لأي سيلز **أونلاين دلوقتي**. لو كذا واحد أونلاين، خد اللي فتح أحدث. ولو مفيش حد أونلاين، اسأل أحمد.
4. **العميل اللي بعت على كذا إعلان لنفس السيلز** يدخل ليد واحد، والريفرنسات كلها في `refAd` مفصولة بفاصلة.
5. **الليد المتكرر** (الرقم موجود في Leads أو Archive، والمقارنة بآخر 9 أرقام، ومعاهم عمود phone2):
   - شوف هو مع مين **دلوقتي** من `LeadState`، مش من owner.
   - لو معاه نفس السيلز صاحب الإعلان: يفضل زي ما هو.
   - لو جه على إعلان سيلز تاني، أو هو في Open to all: يتسحب ويروح لصاحب الإعلان الجديد في حالتين بس: (أ) السيلز اللي معاه ما سجلش ولا أكشن، أو (ب) ميعاد المتابعة (`nextFollow`/`nextAt`) عدّى وما سجلش أكشن جديد بعده. لو السيلز شغال عليه ومتابعته متحدّثة، يفضل معاه.
   - **أحمد بيعمل التحويل بنفسه** من شاشة الأدمن (Transfer) عشان يتسجل في History. فما تعدلش owner في الشيت. اكتب له قايمة باللي محتاجين يتحوّلوا: الاسم، والرقم، ومعاه مين دلوقتي، ويروح لمين، والسبب.
   - الليد اللي ما اتكررش (جه مرة واحدة) والسيستم سحبه من السيلز بقواعده العادية: ما تقربلوش.
6. **الأرقام والمميزات:** ما تخترعش. اللي مش موجود في الملف أو في Daily Ads يفضل فاضي أو «Genral».

## خطوات الرفع
1. نسخة احتياطية من الشيت الأول، بالـ Drive copy_file، والاسم: `Backup <yyyy-mm-dd> before Property Finder leads — The Arabians × Deal Maker`.
2. فلتر الفترة، وحوّل الوقت للقاهرة. شيل اللي من غير رقم، وجمّع العميل اللي جه أكتر من مرة لنفس السيلز.
3. قارن الأرقام بـ `Leads!D` و`Leads!AF` و`Archive!D`، وشوف الموجودين مع مين في `LeadState`.
4. الليدز الجديدة تدخل بـ `append_values` على `Leads!A1`، وقبل كل خانة فيها كلام علامة `'`. الأعمدة بالترتيب:
   `id` (20 حرف ورقم عشوائي small)، `type`=fresh، `name` (أو «no name»)، `phone` (+20…)، `project`، `source`=Property Finder، `region`، `unitType`، `regDate` (تاريخ الليد بتوقيت القاهرة yyyy-mm-dd)، `owner`=id السيلز، `createdAt`=الوقت دلوقتي بالـ ms، `createdBy`=marketer، `status`=فاضي، `nextAt`=0، `nextFollow`=فاضي، `lastBy`=فاضي، `lastAt`=0، `claimedBy`=فاضي، `claimedAt`=0، `handed`=0، `reason`، `prevRep`، `prevComment`، `prevDate`، `dmId` فاضيين، `dmSentAt`=0، `campaign`، `jobTitle`، `qStart` فاضيين، `qIdx`=0، `history`=`[]`، `phone2`=فاضي، `refAd`=الريفرنس، `adPkg`=Premium لو الريفرنس فيه `p` (زي Rnc1p11-509 وRmp11-504) أو Standard لو فيه `s` (زي Rnc1s10-2509).
   - **المشروع:** من Daily Ads بالريفرنس، وبالاسم اللي في تاب `Projects` (مثلاً Taj City → «Taj City MNHD New Cairo»، وIcon → «The Icon Residence New Cairo Style Home»). لو الريفرنس مش موجود: «New Cairo Genral» للي بيبدأ بـ Rnc، و«Mostakbal Genral» للي بيبدأ بـ Rmp، و«Others» لو مفيش ريفرنس.
   - **المنطقة:** Rnc → New Cairo، وRmp → Mostakbal City.
   - **unitType:** من عمود Type في Daily Ads (Apt → Apartment)، ولو مش موجود فاضي.
5. اقرا الصفوف تاني واتأكد إنها اتكتبت صح. بعد حوالي 10 دقايق اتأكد إنها ظهرت في `LeadState` بـ stage s1 مع السيلز الصح (`LeadState!H2` = آخر تحديث).

## التقرير لأحمد (بالعامية)
- عدد الليدز اللي اترفعت، وجدول بالتوزيع: السيلز، والعدد، والعملاء والريفرنس.
- اللي ما اترفعش وليه: من غير رقم، أو موجود قبل كده.
- قايمة التحويلات اللي محتاج يعملها بنفسه (القاعدة 5)، أو «مفيش تحويلات».
- اسم النسخة الاحتياطية.

## سجل الرفع
- 2026-11-07: الفترة من 5/11 لـ 7/11. اترفع 16 ليد جديد (15 بالريفرنس، وMenna من الإيميل راحت لأدهم). ليد واحد ما اترفعش لأنه من غير رقم (Mohamed Eldemenki). أحمد هيحوّل بنفسه لعمار: +201552221877 وRania +201120321330.
