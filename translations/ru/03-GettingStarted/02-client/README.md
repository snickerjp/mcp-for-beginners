<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "2342baa570312086fc19edcf41320250",
  "translation_date": "2025-06-17T15:12:17+00:00",
  "source_file": "03-GettingStarted/02-client/README.md",
  "language_code": "ru"
}
-->
В приведённом выше коде мы:

- Импортировали библиотеки
- Создали экземпляр клиента и подключили его, используя stdio в качестве транспорта.
- Вывели списки prompts, ресурсов и инструментов и вызвали их все.

Вот и всё, клиент, который может взаимодействовать с MCP Server.

Давайте в следующем разделе упражнения подробно разберём каждый фрагмент кода и объясним, что происходит.

## Упражнение: Написание клиента

Как уже говорилось, давайте не будем спешить с объяснением кода, и, конечно, вы можете писать код вместе с нами.

### -1- Импорт библиотек

Импортируем необходимые библиотеки, нам понадобятся ссылки на клиент и выбранный протокол транспорта — stdio. stdio — это протокол для приложений, которые запускаются на вашем локальном компьютере. SSE — другой протокол транспорта, который мы покажем в будущих главах, но это ваш альтернативный вариант. А пока продолжим с stdio.

Давайте перейдём к созданию экземпляров.

### -2- Создание экземпляров клиента и транспорта

Нужно создать экземпляр транспорта и экземпляр клиента:

### -3- Вывод функций сервера

Теперь у нас есть клиент, который может подключаться к серверу при запуске программы. Однако он пока не выводит список доступных функций, давайте сделаем это:

Отлично, теперь мы получили список всех функций. Вопрос — когда их использовать? Этот клиент довольно простой, в том смысле, что нам нужно явно вызывать функции, когда они нужны. В следующей главе мы создадим более продвинутого клиента, который будет иметь доступ к собственной большой языковой модели (LLM). А пока давайте посмотрим, как вызвать функции сервера:

### -4- Вызов функций

Чтобы вызвать функции, нужно убедиться, что мы передаём правильные аргументы и в некоторых случаях — имя вызываемой функции.

### -5- Запуск клиента

Чтобы запустить клиента, введите следующую команду в терминале:

## Задание

В этом задании вы примените полученные знания для создания собственного клиента.

Вот сервер, к которому нужно подключиться через ваш клиентский код, попробуйте добавить больше функций на сервер, чтобы сделать его интереснее.

## Решение

[Решение](./solution/README.md)

## Основные выводы

Основные выводы по этой главе о клиентах:

- Клиенты можно использовать как для обнаружения, так и для вызова функций на сервере.
- Клиенты могут запускать сервер вместе с собой (как в этой главе), но также могут подключаться к уже запущенным серверам.
- Клиенты — отличный способ протестировать возможности сервера наряду с такими альтернативами, как Inspector, который описывался в предыдущей главе.

## Дополнительные ресурсы

- [Создание клиентов в MCP](https://modelcontextprotocol.io/quickstart/client)

## Примеры

- [Java калькулятор](../samples/java/calculator/README.md)
- [.Net калькулятор](../../../../03-GettingStarted/samples/csharp)
- [JavaScript калькулятор](../samples/javascript/README.md)
- [TypeScript калькулятор](../samples/typescript/README.md)
- [Python калькулятор](../../../../03-GettingStarted/samples/python)

## Что дальше

- Далее: [Создание клиента с LLM](/03-GettingStarted/03-llm-client/README.md)

**Отказ от ответственности**:  
Этот документ был переведен с помощью сервиса автоматического перевода [Co-op Translator](https://github.com/Azure/co-op-translator). Несмотря на наши усилия по обеспечению точности, просим учитывать, что автоматический перевод может содержать ошибки или неточности. Оригинальный документ на его исходном языке следует считать авторитетным источником. Для получения критически важной информации рекомендуется обращаться к профессиональному переводу, выполненному человеком. Мы не несем ответственности за любые недоразумения или неправильные толкования, возникшие в результате использования данного перевода.