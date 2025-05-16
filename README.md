# user-service

ТУТ ПЕТЯ ДОЛЖЕН ЖЕСТКО НАПИСАТЬ МИКРОСЕРВИС НА ГО

Какие задачи должен решать Петя, приходят токены в которых ОБЯЗАТЕЛЬНО хранится следующая информация

eyJhbGciOiJSUzI1NiIsImtpZCI6IjY2MGVmM2I5Nzg0YmRmNTZlYmU4NTlmNTc3ZjdmYjJlOGMxY2VmZmIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiI4OTQxODE3NzgxNjEtNXJ0aDI0bTUwZnNpMmJqNGg4OTQyN3BmYWVtdmZrNTIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiI4OTQxODE3NzgxNjEtNXJ0aDI0bTUwZnNpMmJqNGg4OTQyN3BmYWVtdmZrNTIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTcwNjIyMDM3MDQyMTkxMjg3ODYiLCJlbWFpbCI6ImxlYXJwY3NAZ21haWwuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImF0X2hhc2giOiIxeUEwbXpZSVRqYnFEZFg5Nmp4SzN3IiwibmFtZSI6ItCY0LLQsNC9INCg0L7QtNC40L0iLCJwaWN0dXJlIjoiaHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EvQUNnOG9jSVJoY3RPNmppa2w3Q1U2ZF96SXh2aDNsYVhvRk1pTkJwWlhIcTZYTzNlRXhIRHJnPXM5Ni1jIiwiZ2l2ZW5fbmFtZSI6ItCY0LLQsNC9IiwiZmFtaWx5X25hbWUiOiLQoNC-0LTQuNC9IiwiaWF0IjoxNzQ3MzcyMDg5LCJleHAiOjE3NDczNzU2ODl9.ouDc2VHqfvHR9S1FGuwG_3GRUlcK9WS7_oDvTxWOUH-sM8Ga83P8Myg0TXFDKuDEVdIKhd-VaxKWzlc_ZDcE2Giu2X0nH6vqFF6TH4U9PS0DqBM44fPkBW4z758ln3k3lmfAUQ20YpwWoZbEGDpWjvkbsH_j1XtlCrPuG8RGTaVzCkH1MZP6D9VMokaAAlGRnJCcwC3oauNzcTlTy-nq2eCwzMrk-g60eYxOhODfqtL0GO6kv55pUADQBVmEQIGrA_alyA8DTX3-ruFs9-MTZB981SWV_gLJLVVJIDJ0gyERYCVv2cKTRuwLNRuMMY4VO3-lX57qrdgxqaXtM6BfAA

пускаем это дело в jwt.io

{
  "iss": "https://accounts.google.com",
  "azp": "894181778161-5rth24m50fsi2bj4h89427pfaemvfk52.apps.googleusercontent.com", //айди клиента который выпустил этот токен
  "aud": "894181778161-5rth24m50fsi2bj4h89427pfaemvfk52.apps.googleusercontent.com",
  "sub": "117062203704219128786", //уникальный идентификатор пользователя в Google (постоянный для этого аккаунта)
  "email": "learpcs@gmail.com",
  "email_verified": true,
  "at_hash": "1yA0mzYITjbqDdX96jxK3w", //хеш access token'а (используется для валидации)
  "name": "Иван Родин",
  "picture": "https://lh3.googleusercontent.com/a/ACg8ocIRhctO6jikl7CU6d_zIxvh3laXoFMiNBpZXHq6XO3eExHDrg=s96-c",
  "given_name": "Иван",
  "family_name": "Родин",
  "iat": 1747372089,
  "exp": 1747375689
}

Важные параметры которые надо учитывать как по мне 

{
  "sub": "117062203704219128786" //sub - это уникальный айди, будем считать что все sub независимо от провайдера не пересекаются
  "email": "learpcs@gmail.com" //логин типа наш
  "picture": "https://lh3.googleusercontent.com/a/ACg8ocIRhctO6jikl7CU6d_zIxvh3laXoFMiNBpZXHq6XO3eExHDrg=s96-c" //аватарочка которую надо бы в минио загрузить
  "iat": 1747372089, //когда токен сделали
  "exp": 1747375689 //когда токен истекает (по этой штуке будем понимать, истек токен или нет)
}

Пример гугловского токена

Что хочется хранить про пользователя.

В бд должно быть

1. айди - sub
2. email - строка
3. айди аватарки - строка (пока что)
4. строка список ролей через запятую - (например, админ, модератор, дебил) (тоже не хочется так лучше обсудить это)
5. Флаг удаленности аккаунта

Что долнжо быть в апи

1. Регистрация пользователя по токену. (Опционально сделать) Если у человека есть картинка, то ее надо загрузить в минио и потом айди указать на нее.
2. Удалить пользователя по токену.
3. Проверить право пользователя по токену. Немного дебильная фигня, ибо почему бы права не положить в токен? Вы как считаете?

   Почему я говорю что дебильная фигня - хочется, чтобы этот user-service был полностью изолирован от логики токенов. То есть он не должен уметь проверять валиден токен или нет, есть ли какие-то права в токене или нет. Но это банально дольше работает. Че думаете
