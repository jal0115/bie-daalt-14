\# Эргэцүүлэл — Бие даалт 14



\## 1. Хамгийн үнэ цэнэтэй assertion?



Миний хувьд \*\*business rule assertion\*\* хамгийн үнэ цэнэтэй санагдсан. Жишээ нь `Get product by id` request-д бичсэн `pm.expect(d.price).to.be.above(0)` буюу үнэ нь эерэг тоо байх ёстой гэсэн дүрэм. Status 200 шалгах нь хялбар бөгөөд жижигхэн алдаа гаргахад тусалдаг боловч, бизнесийн логикийн дүрэм зөрчигдвөл API "хэвийн ажилласан" мэт харагдаж, гэхдээ data integrity эвдрэх боломжтой. Үнэ сөрөг утгатай product database-д орох нь технический алдаа биш — гэхдээ бизнес шийдвэрт ноцтой нөлөөтэй.



Бас `pm.expect(d.email).to.match(/@/)` ч мөн адил үнэ цэнэтэй — schema-д "email" гэж нэрлэсэн талбар нь жинхэнэ email биш утгатай байж болзошгүй.



\## 2. Negative test жишээ — дэлгэрэнгүй



`Errors → Get not found` request-д `GET /products/9999999` явуулна. Энэ нь хоёр зүйлийг нэгэн зэрэг шалгана:



1\. \*\*Status code:\*\* 404 буцаагдах ёстой (200 биш!)

2\. \*\*Error message:\*\* Response body-д `message` талбар орсон байх



Энэ нь ямар алдаа олох вэ?



\- \*\*Silent failure:\*\* API нь буруу id-д 200 + хоосон object буцаавал, client-side error боловсруулалт буруу ажиллана. Хэрэглэгчид "product байна" гэж буруу сэтгэгдэл үлдээх боломжтой.

\- \*\*Information leakage:\*\* Error message-д тодорхой бус, эсвэл хэт дэлгэрэнгүй (stack trace гэх мэт) мэдээлэл байх боломжтой.

\- \*\*HTTP semantics:\*\* RESTful API-н гол зарчмыг (тохирох status code ашиглах) баримтлаагүй гэдгийг илрүүлнэ.



\## 3. Postman vs Newman зөрүү



Анх Newman локалд ажиллуулахад \*\*бүх 21 тест нэг дор pass болсон\*\* (21/21, 0 fail). Энэ нь миний chain зөв ажиллаж байгааг харуулсан — Login request эхэлж явснаар `token` болон `userId` environment-д хадгалагдаж, дараагийн request-ууд тэдгээрийг ашигласан.



Гэхдээ Postman-аас Newman руу шилжүүлэхэд \*\*нэг асуудал\*\* анхаарал татсан: `env.dev.json`-д Postman нь `token`-ы утгыг автоматаар хоосон болгож экспорт хийсэн (secret шиг гэж үзсэн байж магадгүй). Тиймээс Newman ажиллахаас өмнө `baseUrl`-г гараар нэмэх шаардлагатай болсон. Postman дотор гараар тестлэх үед "current value" нь session дотор хадгалагдсан байсан учир ийм асуудал гарахгүй байсан.



\## 4. Token зохицуулалт (env-ийн соёл)



`env.dev.json` болон `env.ci.json` хоёрт \*\*`token`-ы утгыг `REPLACE\_THIS`\*\* гэсэн placeholder болгож хадгалсан. Жинхэнэ JWT-ыг repository-д commit хийгээгүй — GitHub дээр public repo бол энэ нь шууд security риск.



Гэхдээ `dummyjson.com` нь test environment учир хатуу secret хэрэгтэй биш. Production орчинд бол GitHub Secrets ашиглан:



\- Repo Settings → Secrets and variables → Actions → New secret

\- Жнь `DUMMYJSON\_TOKEN` гэж нэр өгөх

\- workflow yml-д `${{ secrets.DUMMYJSON\_TOKEN }}` ашиглаж env.ci.json доторх `REPLACE\_THIS`-ийг substitute хийх



Манай collection нь `Login` request явснаар token-ыг \*\*runtime-д\*\* шинэчилдэг учир placeholder ажиллах боломжтой.



\## 5. Хамгийн эмзэг газар



API-ийн \*\*response schema\*\* өөрчлөгдвөл миний шалгасан тестүүдийн дийлэнх эвдрэх боломжтой. Жишээ нь:



\- `Login` response-д `accessToken` → `access\_token` (snake\_case) болсон бол chain эвдэрч, дараагийн бүх auth-зависимый request fail болно.

\- `products` array → `items` болсон бол `List products`-ын schema test fail.

\- `price` (number) → string ("9.99") болсон бол type assertion fail.



\*\*Үүнийг бууруулах арга:\*\*



1\. \*\*JSON Schema файл гадаа хадгалах\*\* — `schemas/product.json` гэх мэт, нэг газраас reuse хийх.

2\. \*\*Критик field-үүдэд анхаарал хандуулах\*\* — id, status зэрэг primary identifier-уудыг "lock" хийх; secondary field-уудыг зөвхөн оршин буй эсэхийг шалгах.

3\. \*\*Contract testing\*\* — API provider-тай хамтран schema-ыг гэрээ болгох. Pact гэх мэт хэрэгсэл ашиглаж болно.

4\. \*\*Versioning\*\* — API өөрчлөгдөхөд `/v1/`, `/v2/` гэх мэт версионтэй байх.

5\. \*\*Snapshot testing\*\* — response shape-ийг тогтворжуулах, тогтмол ажиллуулж зөрүү хайх.



Энэ туршлагаас сурсан зүйл — тестийн pyramid дотор API-ын integration test нь нэгээс олон давхаргад ач холбогдол бүхий бөгөөд, тэр нь зөвхөн "ажиллаж байна уу" биш "хэрхэн ажиллаж байна" гэдгийг хариулдаг.

