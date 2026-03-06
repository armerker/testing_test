# Тест-кейсы: GET /products/{productId}/status

## Раздел 1: Позитивные сценарии (Happy Path)

| ID | Название | Шаги | Ожидаемый результат |
|-----|----------|------|--------------------|
| TC-01 | Проверка получения статуса "Готов" при передаче всех параметров | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=true&owner=Создатель&region=Северо-Запад | 200 OK, {"productStatus": 1} |
| TC-02 | Проверка получения статуса "Не готов" при передаче всех параметров | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=false&owner=Пользователь&region=Сибирь | 200 OK, {"productStatus": 0} |
| TC-03 | Проверка обработки параметров со значением null | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=null&owner=null&region=Поволжье | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |
| TC-04 | Проверка работы API при отсутствии всех необязательных параметров | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |

---

## Раздел 2: Авторизация и Аутентификация

| ID | Название | Шаги | Ожидаемый результат |
|-----|----------|------|---------------------|
| TC-05 | Проверка отклонения токена недостаточной длины (15 символов) | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnO | 401 Unauthorized |
| TC-06 | Проверка отклонения токена избыточной длины (17 символов) | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOpQ | 401 Unauthorized |
| TC-07 | Проверка отклонения токена состоящего только из букв | 1. Отправить GET запрос на /products/123/status?authToken=ABCDEFGHIJKLMNOP | 401 Unauthorized |
| TC-08 | Проверка отклонения токена состоящего только из цифр | 1. Отправить GET запрос на /products/123/status?authToken=1234567890123456 | 401 Unauthorized |
| TC-09 | Проверка отклонения токена содержащего спецсимволы | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlM!@# | 401 Unauthorized |
| TC-10 | Проверка отклонения запроса без параметра authToken | 1. Отправить GET запрос на /products/123/status | 401 Unauthorized |
| TC-11 | Проверка отклонения запроса с пустым значением authToken | 1. Отправить GET запрос на /products/123/status?authToken= | 401 Unauthorized |
| TC-12 | Проверка отклонения несуществующего токена валидного формата | 1. Отправить GET запрос на /products/123/status?authToken=NonExistent16Char | 401 Unauthorized |

---

## Раздел 3: Проверка параметров (Негатив)

| ID | Название | Шаги | Ожидаемый результат |
|-----|----------|------|---------------------|
| TC-13 | Проверка обработки строкового значения в параметре recalculate | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=abc | 500 Internal Server Error, тело ответа содержит {"errorMessage": "..."} |
| TC-14 | Проверка обработки неизвестного значения в параметре owner | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&owner=НеизвестныйРоль | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |
| TC-15 | Проверка обработки значения region с пробелом вместо дефиса | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&region=северо запад | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |
| TC-16 | Проверка обработки значения region отсутствующего в допустимом списке | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&region=ДальнийВосток |200 OK, {"productStatus": <значение соответствует статусу продукта>}|

---

## Раздел 4: Граничные случаи и ошибки

| ID | Название | Шаги | Ожидаемый результат |
|-----|----------|------|---------------------|
| TC-17 | Проверка обработки несуществующего идентификатора продукта | 1. Отправить GET запрос на /products/999999/status?authToken=AbCdEfGhIjKlMnOp | 404 Not Found |
| TC-18 | Проверка обработки нулевого идентификатора продукта | 1. Отправить GET запрос на /products/0/status?authToken=AbCdEfGhIjKlMnOp | 404 Not Found |
| TC-19 | Проверка обработки отрицательного идентификатора продукта | 1. Отправить GET запрос на /products/-1/status?authToken=AbCdEfGhIjKlMnOp | 404 Not Found |
| TC-20 | Проверка обработки некорректного идентификатора продукта (буквы) | 1. Отправить GET запрос на /products/abc/status?authToken=AbCdEfGhIjKlMnOp | 404 Not Found |
| TC-21 | Проверка обработки неподдерживаемого HTTP-метода | 1. Отправить POST запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp | 405 Method Not Allowed |

---

## Раздел 5: Комбинаторика (Pairwise тесты)

| ID | Название | Шаги | Ожидаемый результат |
|-----|----------|------|---------------------|
| TC-22 | Проверка комбинации: recalculate=true, owner=Пользователь, region=Поволжье | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=true&owner=Пользователь&region=Поволжье | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |
| TC-23 | Проверка комбинации: recalculate=false, owner=Создатель, region отсутствует | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=false&owner=Создатель | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |
| TC-24 | Проверка комбинации: recalculate=null, owner отсутствует, region=Северо-Запад | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=null&region=Северо-Запад | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |
| TC-25 | Проверка комбинации: recalculate отсутствует, owner=null, region=Сибирь | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&owner=null&region=Сибирь | 200 OK, {"productStatus": <значение соответствует статусу продукта>} |

---

## Раздел 6: Дополнительные проверки

| ID    | Название                                                    | Шаги                                                                                                   | Ожидаемый результат                                            |
|-------|-------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| TC-26 | Проверка обработки лишнего параметра в запросе              | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=true&extra=test | 200 OK, {"productStatus": <значение соответствует статусу продукта>} (API игнорирует лишние параметры) |
| TC-27 | Проверка обработки параметра recalculate в верхнем регистре | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&recalculate=TRUE            | 200 OK, {"productStatus": <значение соответствует статусу продукта>} (API регистронезависим)          |
| TC-28 | Проверка обработки параметра owner с маленькой буквы        | 1. Отправить GET запрос на /products/123/status?authToken=AbCdEfGhIjKlMnOp&owner=создатель             | 200 OK, {"productStatus": <значение соответствует статусу продукта>} (API регистронезависим)      |
| TC-29 | Проверка обработки максимального значения productId         | 1. Отправить GET запрос на /products/2147483647/status?authToken=AbCdEfGhIjKlMnOp                      | 404 Not Found (если продукт не существует)                     |
| TC-30 | Проверка обработки очень большого значения productId        | 1. Отправить GET запрос на /products/10000000000/status?authToken=AbCdEfGhIjKlMnOp                     | 400 Bad Request (число выходит за пределы int)                 |
| TC-31 | Проверка обработки отрицательного значения productId        | 1. Отправить GET запрос на /products/-99999999999/status?authToken=AbCdEfGhIjKlMnOp                    | 400 Bad Request (отрицательное значение за пределами допустимого диапазона)                                               |