# Lab05 - API систем тест: Postman + Newman

## Оюутны мэдээлэл

- Оюутны код: B232270032
- Лабораторийн ажил: Lab05
- Сэдэв: API систем тест - Postman + Newman

## Ашигласан орчин

- Node.js: v18.19.1
- Newman: 6.2.2
- API: http://localhost:3000
- Тестийн хэрэгсэл: Postman, Newman
- Үйлдлийн орчин: WSL Ubuntu

## Тестийн оролтын ангилал

| Оролт | Ангилал | Төлөөлөх утга |
|---|---|---|
| studentID | Идэвхтэй оюутан | B232270032 |
| studentID | Идэвхгүй оюутан | B232270032, status=inactive |
| studentID | Байхгүй оюутан | B999999999 |
| coursesTaken | Шаардлага хангасан | ["CS201"] |
| coursesTaken | Шаардлага хангаагүй | [] |
| courseID | Байгаа хичээл | CS313 |
| courseID | Байхгүй хичээл | CS999 |
| prerequisites | Бүгдийг үзсэн | ["CS201"] |
| prerequisites | Шаардлагагүй | [] |
| request field | courseID байхгүй | studentID only |
| JSON | Буруу JSON | хаалтын } тэмдэггүй |

## Тестийн спецификаци

| № | Тест | Нөхцөл | Expected status | Expected result |
|---|---|---|---|---|
| 1 | Happy Path | Active student, CS201 үзсэн, CS313 prerequisite=CS201 | 201 | OK |
| 2 | No Student | studentID системд байхгүй | 200 | ERROR_NO_STUDENT |
| 3 | Inactive Student | Оюутны status inactive | 200 | ERROR_INACTIVE_STUDENT |
| 4 | No Course | courseID системд байхгүй | 200 | ERROR_NO_COURSE |
| 5 | Missing Prerequisites | Active student боловч CS201 үзээгүй | 200 | ERROR_PREREQUISITES |
| 6 | Bad Request | courseID талбар байхгүй | 400 | ERROR_BAD_REQUEST |
| 7 | Bad JSON | JSON синтакс зориуд буруу | 400 | ERROR_BAD_JSON |
| 8 | No Prerequisites | prerequisites=[] хичээл | 201 | OK |

## PASS тест

Зөв oracle-уудтай `lab05-collection.json` collection-ийг Newman-аар ажиллуулсан.

Үр дүн:

- Requests: 18, failed: 0
- Assertions: 16, failed: 0
- Exit code: 0
- Evidence: `results/newman-pass.txt`

PASS run үед бүх үндсэн тест амжилттай ажилласан.

## FAIL тест

`lab05-collection-fail.json` collection-д Happy Path тестийн зөв 201 status oracle-ийг зориуд 200 болгон өөрчилж assertion failure үүсгэсэн.

Үр дүн:

- Requests: 18, failed: 0
- Assertions: 15 passed, 1 failed
- Алдаа: expected status code 200 but got 201
- Exit code: 1
- Evidence: `results/newman-fail.txt`

Энэ нь oracle бодит үр дүн болон хүлээгдэж буй үр дүн зөрөхөд алдааг илрүүлж байгааг харуулсан.

## DOWN тест

API серверийг Ctrl+C ашиглан зогсоож Newman collection-ийг дахин ажиллуулсан.

Үр дүнд:

- `connect ECONNREFUSED 127.0.0.1:3000` алдаа гарсан.
- Exit code: 1
- Evidence: `results/newman-down.txt`

Энэ нь сервер ажиллахгүй үед request API interface-д хүрч чадахгүй байгааг харуулсан. DOWN тестийн connection error нь буруу expected result-оос үүссэн oracle failure биш, сервертэй холбогдож чадаагүй interface/request error юм.

## Дүгнэлт

Энэ лабораторийн ажлаар Postman болон Newman ашиглан API системийн тест хийж сурлаа. Эхлээд оюутан болон хичээлийн өгөгдлийг PUT хүсэлтээр бэлтгэж, бүртгэлийн POST хүсэлтийг тестэлсэн. Happy Path тестээр зөв нөхцөлд API 201 статус болон OK үр дүн буцааж байгааг шалгасан. Мөн байхгүй оюутан, идэвхгүй оюутан, байхгүй хичээл болон prerequisite хангаагүй нөхцөлүүдийг тус тус тестэлсэн. Missing field болон буруу JSON ашиглан 400 статус буцаах boundary нөхцөлүүдийг шалгасан. Тест бүрд status code болон result утгыг шалгах oracle ашигласан. PASS run үед 16 assertion бүгд амжилттай болж exit code 0 гарсан. FAIL run үед нэг oracle-ийг зориуд буруу тохируулснаар Newman assertion failure илрүүлж exit code 1 буцаасан. DOWN run үед серверийг унтраахад ECONNREFUSED connection error гарч, interface error болон oracle failure хоёрын ялгааг ажигласан. Ингэснээр API тестийг автоматжуулж, Newman-ийн үр дүн болон exit code ашиглан тестийн амжилт, алдааг тодорхойлох дадлага хийлээ.
