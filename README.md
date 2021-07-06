# Авторизация 
Авторизация выполняетья в 2 этапа.
1. Получение кода авторизации
2. Получение токена по коду авторизации
3. Обновление токена авторизации (токен живет 1 час)

**Инструкции**:

1, 2 - https://wp-oauth.com/docs/general/grant-types/authorization-code/<br>
3 - Обновление токена: https://wp-oauth.com/docs/general/grant-types/refresh-token/

PS: В инструкции после завершения 2х этапов предлагают вариант тестирования запроса к API.
Вместо него использовать:

method : POST
url: https://develop.80-ballov.ru/oauth/test

vars: `access_token`: [токен доступа]

####  Пример запроса:

https://80-ballov.ru/oauth/test?access_token=lycngl8s64pvquz8lve614icw5fqxij7rwr9ehwk

Пример ответа:

    {
        id :  11 ,
        access_token :  lycngl8s64pvquz8lve614icw5fqxij7rwr9ehwk ,
        client_id :  mZzSMLTpedjC1K5tw8pWfbaCyUg7bPpv03Mh2lwd ,
        user_id :  217784 ,
        expires : 1616593903,
        scope :  basic ,
        ap_generated : null
    }



# План:

Инструментарий по которому можно будет взаимодействовать с данными сайта методом REST

	GET — Получить данные
	POST — Создать данные
	PUT — Изминить данные

####  Сущности с которыми предоставляем взаимодействие:
	
* Пользователь
* Преподаватель
* Курс
* Категории курса
* Платежи
* Рейтинг
* Оплата


#### Подробней:
* **Пользователь:**
    - GET
        - [user](https://github.com/AndryWJ/mobAPIDoc#user) - `Получение пользователя`
        - [user_list](https://github.com/AndryWJ/mobAPIDoc#user_list) - `Получение пользователей`
        - [user_courses](https://github.com/AndryWJ/mobAPIDoc#user_courses) - `Получение доступных курсов пользователя`
        - [user_curators](https://github.com/AndryWJ/mobAPIDoc#user_curators) - `Получение активных курраторств у пользователя`
    - POST
        - [user_register](https://github.com/AndryWJ/mobAPIDoc#user_register) - `Регистрация пользователя`
    - PUT
        - [user_change](https://github.com/AndryWJ/mobAPIDoc#user_change) - `Изминение данных пользователя`
    - AUTH2 (Как и админский профиль для рабоыт с API можно авторизировать и обычных пользователей)
* **Преподаватели:**
    - GET
        - [teacher_list](https://github.com/AndryWJ/mobAPIDoc#teacher_list) - `Получение преподавателей`
* **Категории курса:**
    - GET
        - [category_list](https://github.com/AndryWJ/mobAPIDoc#category_list) - `Получение категорий курса`
* **Курс:**
    - GET
        - [course](https://github.com/AndryWJ/mobAPIDoc#course) - `Получение одного курса`
        - [course_list](https://github.com/AndryWJ/mobAPIDoc#course_list) - `Получение курсов`
    - PUT
        - [set_answer](https://github.com/AndryWJ/mobAPIDoc#set_answer) - `Запись ответа по заданию`
        - [set_watched](https://github.com/AndryWJ/mobAPIDoc#set_watched) - `Установка прогреса по просмотрам видео`
* **Платежи:**
    - GET
        - [pay_list](https://github.com/AndryWJ/mobAPIDoc#pay_list) - `Получить платежи пользователя`
        - [order_variants](https://github.com/AndryWJ/mobAPIDoc#order_variants) - `Курсы доступные для покупки`
        - [promocode](https://github.com/AndryWJ/mobAPIDoc#promocode) - `Получить промокоды`
    - POST
        - [pay_link](https://github.com/AndryWJ/mobAPIDoc#pay_link) - `Создать и возвратить ссылку на оплату курсов, передавать надо данные пользователя и курсы которые он выбрал`
* **Прогресс:**
    - GET
        - [get_rating](https://github.com/AndryWJ/mobAPIDoc#get_rating) - `Получение рейтинга (по умолчанию все, либо параметром указать за какой из 3х периодов получить рейтинг)`
        - [get_progress_user](https://github.com/AndryWJ/mobAPIDoc#get_progress_user) - `Получить прогресс пользователя (передавать ID категории и ID пользователя, еще не смотрел как переделали сайдбар и по чему его можно получать)`





# Роуты:

URL: https:exemple_domain/oauth/**[rout_name]**


## user
Получить пользователя

#### Список полей пользователя:
- `first_name` - Имя
- `last_name` - Фамилия
- `user_login` - Логин
- `email` - Email пользователя
- `user_registered` - Дата регистрации



**ПРИНИМАЕТ:**
**method:** GET

| переменные | по умолчанию| описание  |
| :---------- |:---------:|:---------:|
| user_id *(обязательно)*      | null  | ID пользователя которого хотим получить |
| fields *(опционально)* | null | Поля которые нужно получить|

**ВОЗВРАЩАЕТ :**
Успех

    [
        {
            "user_id": "777777",
            "email": "test@test.net",
            "user_login": "test"
            ...
        }
    ]

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |

## user_list
Получить список пользователей

**ПРИНИМАЕТ:**

**method:** GET

| переменные | по умолчанию| описание  |
| :---------- |:---------:|:---------:|
| number | 10 | Количество пользователей которых нужно получить|
| offset | 0 | Отступ от начала полученного списка|
| fields | `['user_id','email','user_login']` | Поля которые нужно получить|


**ВОЗВРАЩАЕТ :**

Успех (Массив пользователей)


| переменная | описание  |
| ---------- |:---------:|
| `count_users`    | Количество пользователей |
| `users`    | Выбраные пользователи |

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |

## user_register
Регистрация пользователя

**ПРИНИМАЕТ:**

**method:** POST

| переменные | по умолчанию| описание  |
| :---------- |:---------:|:---------:|
| name *(обязательно)*      | null  | Имя для нового пользователя (мин. 2 символа) |
| user_email *(обязательно)* | null | Email для нового пользователя - на этот адрес будет отправлен пароль для входа|

**ВОЗВРАЩАЕТ :** JSON со свойствами:

Успех

| переменная | описание  |
| ---------- |:---------:|
| `user_id`    | id пользователя|

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## user_change

**method:** PUT
| переменные | по умолчанию| описание  |
| :---------- |:---------:|:---------:|
| `user_id` | `777` | id пользователя которого будем менять |
| `fields` | `["email":"newemail@test.com","user_city":"USA"]` | Поля и им присваемые значения |

#### Список доступных для изменения полей:
- `first_name` - Имя
- `last_name` - Фамилия
- `email` - email пользователя
- `user_class` - Класс пользователя
- `user_city` - Город
- `user_phone` - Телефон

Успех

| переменная | описание  |
| ---------- |:---------:|
| `message`    | Данные пользователя [user_id] обновлены|

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## teacher_list
Преподаватели

**ПРИНИМАЕТ:**

**method:** GET

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `id` *(опционально)* | null | id преподавателя которого нужно получить. Если не указано отдаст список
|`limit`| 5 |Количество|
|`offset`| 0 |Отступ от первого преподавателя|
|`fields`| 'all' |Поля которые нужно получить, возможные значения смотреть ниже (если нужно несколько полей можно передать массив)|

#### Список полей преподавателя:
- `id` - id преподавателя
- `name` - ФИО преподавателя
- `description` - id преподавателя
- `mission` - преподаваемые предметы/должность преподавателя
- `foto` - фото
- `foto_black` - фото (чёрно-белое)
- `video_welcome` - Видео "Знакомство"
- `predmetAboutExam` - Видео "Об экзамене"
- `social_inst` - Ссылка на Instagram
- `social_vk` - Ссылка на ВК
- `social_youtube` - Ссылка на Youtube
- `expert_ege` - Действующий эксперт ЕГЭ `true|false`
- `expert_ege_text` - Текст действующего эксперта ЕГЭ
- `learner_up_100` - Более 100 учеников подготовлено `true|false`
- `learner_up_200` - Более 200 учеников подготовлено `true|false`
- `learner_up_300` - Более 300 учеников подготовлено `true|false`
- `learner_up_400` - Более 400 учеников подготовлено `true|false`
- `teacher_of_the_year` - Победитель премии «Учитель года» `true|false`
- `teacher_of_the_year_text` - Текст победителя премии «Учитель года»
- `teacher_sweet` - Победитель премии портала «80 Баллов» в номинации самый милый преподаватель. `true|false`
- `lesson_up_200` - Курс содержит более 200 видео по предмету `true|false`
- `isset_100_balls` - Есть стобалльники `true|false`
- `100_balls_text` - Текст стобальников
- `teacher_100_balls` - Сдал ЕГЭ на 100 баллов `true|false`
- `teacher_red_diplom` - Окончил ВУЗ с красным дипломом `true|false`


## user_curators
Активное кураторство у пользователя

**ПРИНИМАЕТ:**

**method:** GET

**ВОЗВРАЩАЕТ :**

Успех

id курсов у которых активно кураторство и тип покупки
`full_pay` - полная оплата
`recurrent_pay` - подписка
{
    "math-ege": "full_pay",
    "history-oge": "recurrent_pay",
    "geografija-ege": "full_pay"
}

Неудача
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## user_courses
Курсы доступные пользователю

**ПРИНИМАЕТ:**

**method:** GET

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `group_examen` *(опционально)* | false | групировать ли курсы в ответе по типу экзамена

**ВОЗВРАЩАЕТ :** JSON со свойствами:

Успех

Пример ответа на запрос курсов для пользоватлей с id 160905 и 123:

    {
        "160905": [
            {
                "ID": "biology-oge",
                "name": "Биология ОГЭ 2021",
                "examen": "oge",
                "link": "/video_page/privetstvie-biologija-ogje/"
            },
            {
                "ID": "chemistry-ege",
                "name": "Химия ЕГЭ 2021",
                "examen": "ege",
                "link": "/video_page/privetstvie-himiya-ege/"
            },
            {
                "ID": "math-ege",
                "name": "Математика ЕГЭ 2021",
                "examen": "ege",
                "link": "/video_page/privetstvie-matematika-egje/"
            },
            {
                "ID": "history-oge",
                "name": "История ОГЭ 2021",
                "examen": "oge",
                "link": "/video_page/privetstvie-istorija-ogje/"
            },
            {
                "ID": "geografija-ege",
                "name": "География ЕГЭ 2021",
                "examen": "ege",
                "link": "/video_page/privetstvie-geografija-egje/"
            }
        ],
        "123": []
    }

То же самое с параметром `group_examen = true` :
    
    {
        "160905": {
            "oge": [
                {
                    "ID": "biology-oge",
                    "name": "Биология ОГЭ 2021",
                    "examen": "oge",
                    "link": "/video_page/privetstvie-biologija-ogje/"
                },
                {
                    "ID": "history-oge",
                    "name": "История ОГЭ 2021",
                    "examen": "oge",
                    "link": "/video_page/privetstvie-istorija-ogje/"
                }
            ],
            "ege": [
                {
                    "ID": "chemistry-ege",
                    "name": "Химия ЕГЭ 2021",
                    "examen": "ege",
                    "link": "/video_page/privetstvie-himiya-ege/"
                },
                {
                    "ID": "math-ege",
                    "name": "Математика ЕГЭ 2021",
                    "examen": "ege",
                    "link": "/video_page/privetstvie-matematika-egje/"
                },
                {
                    "ID": "geografija-ege",
                    "name": "География ЕГЭ 2021",
                    "examen": "ege",
                    "link": "/video_page/privetstvie-geografija-egje/"
                }
            ]
        },
        "123": []
    }


Неудача
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |



## category_list
Категории курсов

**ПРИНИМАЕТ:**

**method:** GET

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `parent` *(опционально)* | null | id родительской категории. Если не указано вернет корневой уровень
| `group_examen` *(опционально, только для корневого уровня категорий)* | false | Групировать ли по типу экзамена ЕГЄ/ОГЄ
| `number` *(опционально)* | '0' | Количество получаемых категорий, по умолчанию все.
| `offset` *(опционально)* | '0' | Отступ от первой категории, по умолчанию без отступа


**ВОЗВРАЩАЕТ:** JSON со свойствами:

Успех

    [
        {
            "736": {
                "term_id": 736,
                "name": "Видеокурс",
                "description": "",
                "examen": null,
                "link": "https://80-ballov.ru/video_category/videokurs/"
            },
            "714": {
                "term_id": 714,
                "name": "Педагогический курс",
                "description": "",
                "examen": null,
                "link": "https://80-ballov.ru/video_category/pedagogicheskij-kurs/"
            }
            ...
        }
    ]

Неудача
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## course
Возвращает один курс по id

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `course_id` *(обязательно)* | null | id курса который нужно получить
| `fields` *(опционально)* | `['ID','content','title','type_course']` | возвращаемые поля

Успех
    [
        {
            "ID": 157943,
            "content": "",
            "title": "Тренировочный тест (часть 4)",
            "type_course": "test"
        }
    ]

Неудача
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## course_list
Список курсов по id категории

**ПРИНИМАЕТ:**

**method:** GET

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `term_id` *(обязательно)* | null | id категории курсы из которой нужно получить
| `number` *(опционально)* | '0' | Количество получаемых курсов, по умолчанию все.
| `offset` *(опционально)* | '0' | Отступ от первого курса, по умолчанию без отступа
| `order` *(опционально)* | 'DESC' | Направление сортировки DESC|ASC
| `fields` *(опционально)* | `['ID','content','title','type_course']` | возвращаемые поля

**ВОЗВРАЩАЕТ:**:

Успех

    [
        {
            "ID": 163277,
            "content": "",
            "title": "Контрольный тест (часть 4)",
            "type_course": "test"
        },
        {
            "ID": 163271,
            "content": "",
            "title": "Контрольный тест (часть 3)",
            "type_course": "test"
        },
        {
            "ID": 163270,
            "content": "",
            "title": "Контрольный тест (часть 2)",
            "type_course": "test"
        },
    ]

Неудача
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## set_answer

**ПРИНИМАЕТ:**

**method:** PUT

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `user_id` *(обязательно)* | null | id пользователя для которого записать ответ
| `course_id` *(обязательно)* | null | id курса по которому нужно запистаь ответ
| `test_index` *(обязательно)* | null | номер задания (сейчас у тестов есть от 1 до 5 заданий)
| `answer` *(обязательно)* | null | записываемый ответ

Успех

    {
        "result": "Задание решено неверно",
        "data": {
            "user_id": "169990905",
            "course_id": "9999",
            "answer": "Мой ответ",
            "predmet_id": 15,
            "test_name": "answer_task1",
            "true_answer": ""
        }
    }

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## set_watched

**ПРИНИМАЕТ:**

**method:** PUT

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `user_id` *(обязательно)* | null | id пользователя для которого записать прогресс
| `course_id` *(обязательно)* | null | id курса по которому нужно запистаь прогресс
| `status` *(обязательно)* | null | записываемый прогресс: 1 - просмотрено или 0 - не просмотрено

Успех

    {
        "user_id": "160905",
        "course_id": "877",
        "status": 1,
        "predmet_id": 15,
        "term": 185,
        "result": "Статус успешно изменен!"
    }

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## pay_list

**ПРИНИМАЕТ:**

**method:** GET

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `user_id` *(обязательно)* | null | id пользователя для которого записать прогресс

Успех

    [
        {
            "ID": "math-ege",
            "name": "Математика ЕГЭ 2021",
            "cost": 5990
        },
        {
            "ID": "history-oge",
            "name": "История ОГЭ 2021",
            "cost": 4990
        },
        {
            "ID": "geografija-ege",
            "name": "География ЕГЭ 2021",
            "cost": 5990
        }
    ]

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |


## order_variants

**ПРИНИМАЕТ:**

**method:** GET (Не принимает никаких данных)

Успех

    {
        "math-ege": {
            "name": "Математика ЕГЭ 2022",
            "examen": "ege",
            "cost": 14990,
            "cost_podpiska": 1190,
            "link": "/video_page/privetstvie-matematika-egje/"
        },
        "all-ege": {
            "name": "Премиум ЕГЭ 2022<br><strong>(все доступные предметы ЕГЭ)</strong>",
            "examen": "ege",
            "cost": 29990,
            "cost_podpiska": 9990
        },
        "biology-oge": {
            "name": "Биология ОГЭ 2022",
            "examen": "oge",
            "cost": 7990,
            "cost_podpiska": 1190,
            "link": "/video_page/privetstvie-biologija-ogje/"
        }
    }

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |

## pay_link

**ПРИНИМАЕТ:**

**method:** GET

| переменные |  по умолчанию | описание  |
| :---------- |:---------:|:---------:|
| `bay_predmets` *(обязательно)* | null | План который покупает клиент или несколько, планы доступные для покупки можно получить запросом на роут "[order_variants](https://github.com/AndryWJ/mobAPIDoc#order_variants)" например : `['fizika-ege','russian-ege']` |
| `activate_promokod`  | null | Промокод |
| `kuratorstvo` *(обязательно)* | null | Выбрал ли пользователь кураторство к какому то из планов, доступные значения 'none_pay','recurrent_pay','full_pay' пример : `[ "fizika-ege" => "none_pay", "russian-ege" => "recurrent_pay" ]`  |
| `name` *(обязательно)* | null |  Имя пользователя |
| `lastname` *(обязательно)* | null |  Фамилия пользователя |
| `phone` *(обязательно)* | null |  Телефон пользователя |
| `email` *(обязательно)* | null |  Email пользователя |
| `source` | "Не помню" |  Источник Варианты : `["Не помню","Решу ЕГЭ","Youtube","Из Яндекса","Из Google","Вконтакте ","Инстаграм","От знакомых"]` |

Успех

| переменная | описание  |
| ---------- |:---------:|
| `validateError` | ошибки от банка, если есть |
| `URL`    | ссылка на оплату |

    {
        "validateError":"",
        "URL":"https://securepay.tinkoff.ru/new/07P9EvjE"
    }

Неудача

| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки формирования платежа |



## promocode

**ПРИНИМАЕТ:**

**method:** GET

Успех

    [
        {
            "80-BALLOV": {
                "percent": 30
            },
            "EGE2020": {
                "percent": 50
            },
            "EASY80": {
                "percent": 10
            },
            "math20": {
                "percent": 15
            },
            "sidimdoma": {
                "percent": 12
            },
            "ILOVE-ChitaiGoro": {
                "percent": 30
            },
            "ILOVE-IUROK": {
                "percent": 10
            }
        }
    ]

Неудача
 
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки формирования платежа |

## get_rating

**ПРИНИМАЕТ:**

**method:** GET

| переменная | описание  |
| ---------- |:---------:|
| `period` | За какой период вернуть рейтинг ? возможные значения : `current_month,current_week,current_day` |

Успех

    [
        {
            "user_id": "228794",
            "user_email": "zshtangrat@mail.ru",
            "user_login": "zshtangrat",
            "balls": "3.39348645035849",
            "first_name": "",
            "last_name": "",
            "avatar": "http://1.gravatar.com/avatar/a4c327e20dd0a288e84e70df970488b3?s=96&d=mm&r=g"
        },
        {
            "user_id": "150509",
            "user_email": "misaosipcov@gmail.com",
            "user_login": "id535378145",
            "balls": "2.568981921979059",
            "first_name": "Михаил",
            "last_name": "Осипцов",
            "avatar": "https://lh5.googleusercontent.com/-APt3YwQeoHI/AAAAAAAAAAI/AAAAAAAAAAA/VzRxBAj15KI/photo.jpg?sz=50"
        },
        {
            "user_id": "197062",
            "user_email": "varraa2000@gmail.com",
            "user_login": "varraa2000",
            "balls": "2.093244529019974",
            "first_name": "",
            "last_name": "",
            "avatar": "https://lh3.googleusercontent.com/a-/AOh14GgoNr5MwkZNR-peoHbTma9GdbDdnbMvZI_SKwTO=s96-c?sz=50"
        }
    ]

Неудача
 
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки формирования платежа |



## get_progress_user
Прогресс пользователя - успеваемость пользователя по категории (предмету курса) в балах от 0 - 100, так же отдает id курсов которые просмотрены пользователем и их род. категорию

**ПРИНИМАЕТ:**

**method:** GET

| переменная | описание  |
| ---------- |:---------:|
| `user_id` | id пользователя прогресс которого хотим получить |
| `root_category_id` | id родительской категории(предмета курса) получить можно методом [category_list](https://github.com/AndryWJ/mobAPIDoc#category_list) без указания аргумента `parent`  |


Успех

    [
        {
            "root_category_id": "12",
            "name": "Математика ЕГЭ",
            "balls": "0.28",
            "progress_courses_sidebar": [
                {
                    "viewed": "2021-05-31 08:58:39",
                    "course_id": "877",
                    "category_id": "185"
                },
                {
                    "viewed": "2020-07-28 12:32:01",
                    "course_id": "93356",
                    "category_id": "185"
                },
                {
                    "viewed": "2020-07-28 12:34:14",
                    "course_id": "127166",
                    "category_id": "185"
                },
                {
                    "viewed": "2021-03-16 10:23:37",
                    "course_id": "93375",
                    "category_id": "226"
                },
                {
                    "viewed": "2021-03-16 10:19:08",
                    "course_id": "127168",
                    "category_id": "226"
                },
                {
                    "viewed": "2021-01-12 15:56:27",
                    "course_id": "161021",
                    "category_id": "822"
                },
                {
                    "viewed": "2021-03-16 10:13:41",
                    "course_id": "162297",
                    "category_id": "866"
                },
                {
                    "viewed": "2021-06-29 09:10:18",
                    "course_id": 122114,
                    "category_id": 168
                },
                {
                    "viewed": "2021-06-29 09:12:29",
                    "course_id": 122115,
                    "category_id": 168
                }
            ]
        }
    ]


Неудача
 
| переменная | описание  |
| ---------- |:---------:|
| `error_message`    | Текст ошибки |
