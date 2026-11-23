# Лаборатори 9 

- Баяржавхлан
- B222270019

## Орчин

```text
openjdk version "21.0.13" 2026-10-20 LTS
OpenJDK Runtime Environment Temurin-21.0.13+9 (build 21.0.13+9-LTS)
OpenJDK 64-Bit Server VM Temurin-21.0.13+9 (build 21.0.13+9-LTS, mixed mode, sharing)
```

```text
Apache Maven 3.10.1 (3afd8084b5629f80b97750906a719879ed4cd283)
Maven home: C:\Users\hp\Downloads\apache-maven-3.10.1-bin\apache-maven-3.10.1
Java version: 21.0.13, vendor: Eclipse Adoptium, runtime: C:\Program Files\Eclipse Adoptium\jdk-21.0.13.9-hotspot
Default locale: en_US, platform encoding: UTF-8, time zone: Asia/Shanghai
OS name: "windows 11", version: "10.0", arch: "amd64", family: "windows"
```

## A — Хэвийн урсгалын тест

Хэвийн урсгалыг шалгах эхний 5 тест бүгд алдаагүй ажилласан. `results/jacoco-A.csv`-ээс харахад Line хамралт 14/25 буюу 56.0%, Branch хамралт 18/41 буюу 43.9% гарсан.

JaCoCo-ийн тайланд оролтын шалгалт, `gpa()` дахь хоосон жагсаалтын шалгалт, `scholarship()`-ийн доод түвшний нөхцөлүүд шараар тэмдэглэгдсэн байв. Харин `letterGrade()`-ийн C/D/F салаа,
`gpa()`-ийн switch доторх C/D/F салаа, exception шиддэг мөрүүд болон 70/50/0 хувийн тэтгэлгийн мөрүүд улаанаар үзэгдсэн, өөрөөр хэлбэл огт ажиллаагүй байна.

## B — CFG ба 100% branch хамралт

Буруу оролт, үсгэн дүн бүр, switch-ийн default салаа, мөн `scholarship()` доторх богино холболттой (short-circuit) нөхцөл бүрийг тус тусад нь тестлэсэн. 
Нийт 19 тест хэрэгтэй болсон. `results/jacoco-B.csv`-д `BRANCH_MISSED = 0`, `BRANCH_COVERED = 41`, `LINE_MISSED = 0`, `LINE_COVERED = 25` гарсан нь Branch болон Line хамралт хоёулаа 100% хүрснийг харуулж байна.

![scholarship функцийн CFG](cfg.png)

Мөн энэ зургийг зурж найзаасаа авсан болно тийм учраас ойлгохыг хүсэж байна (Олон зүйл хийх хэрэг гарсан учир зурж амжаагүй)

## C — Давталтын зааг

`gpaRejectsEmptyScores()`, `gpaHandlesSingleScoreLoopBoundary()`, `gpaHandlesManyScoresLoopBoundary()` тестүүд давталтыг 0, 1, 2 ба түүнээс дээш удаа ажиллуулж шалгана.
`gpaRejectsNullScores()` нь null оролтыг тусад нь хариуцна.
Эдгээр тест нь аль хэдийн 100% болсон хамралтын хувийг нэмэгдүүлэхгүй. 
Гэсэн ч хамралтын тоо нь оролтын хэмжээний зааг дээрх үр дүн, exception, давталтын логик зөв эсэхийг баталгаажуулдаггүй. Тиймээс заагийн тохиолдол бүрийг тусад нь шалгах шаардлагатай.

## D — Зааг утгын алдаа

`letterGradeReturnsDAtLowerBoundary()` тестийг нэмэхэд `expected: <D> but was: <F>` гэсэн алдаагаар унасан. Шалтгаан нь D дүнгийн нөхцөл `score > 60` гэж бичигдсэн тул яг 60 оноо F болж байсан явдал юм.
Нөхцөлийг `score >= 60` болгож залруулж, `Fix letterGrade boundary: 60 belongs to D` commit-оор хадгалсан. Үүний дараа нийт 28 тест бүгд ногоон болж, `results/jacoco-final.csv`-д Branch 41/41, Line 25/25 буюу хоёул 100% гарсан.
Branch хамралт 100% байсан ч 60 онооны алдаа илрээгүй. `score > 60` ба `score >= 60` хоёр нь ижил true/false хоёр салаа үүсгэдэг. 
Өмнөх тестүүд хоёр салааг хоёуланг нь дайрсан боловч яг 60 гэсэн заагийн утгыг шалгаагүй байв. Хамралт нь аль код, аль салаа ажилласныг л хэмждэг бөгөөд гарсан үр дүн спецификацтай таарч байгааг батлахгүй. 
Иймд хамралтын хэмжээсийг зааг утгын тест болон зөв хүлээгдэж буй үр дүнтэй (oracle) хослуулан ашиглах нь чухал.

## AI ашиглалт

Codex-ийг даалгаврын зааврыг унших, тестийн хувилбаруудыг төлөвлөх, гарсан үр дүнг шалгахад туслах хэрэгсэл болгон ашигласан.
