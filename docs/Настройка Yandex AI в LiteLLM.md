# Настройка Yandex AI в LiteLLM

Требуется существующее рабочее пространство в [Yandex AI Studio](https://aistudio.yandex.ru/) + значения ID рабочей папки (для примера используется `b1g540utdtsfepohbl4h`) + сгенерированный [API ключ](https://aistudio.yandex.ru/ru/docs/ai-studio/operations/get-api-key).

1. [Запустить LiteLLM](../litellm/README.md) и открыть в браузере http://localhost:4000 и авторизоваться:
    - **Пользователь:** admin
    - **Папроль:** Смотри параметр мастер ключа `LITELLM_MASTER_KEY` в [litellm/.env](/litellm/.env)
2. Заходим в раздел `Models + Endpoints`: http://localhost:4000/ui/models-and-endpoints
3. Создаем данные для аутентификации.

    Выбираем пункт `LLM Credinals`, затем справа `+ Add Credinal` и указываем параметры:
    - **Credinal Name:** Yandex AI
    - **Provider:** Open AI
    - **API Base:** https://ai.api.cloud.yandex.net/v1
    - **OpenAI Organization ID:** значение ID рабочей папки
    - **OpenAI API Key:** сгенерированный API ключ

    Пример заполнения:

    ![Пример заполнения](/docs/images/yandex_cred.png)

    Завершаем создание нажатием на кнопку `Add credinals`.

4. Добавляем модель.

    Для пример возьмем [GPT OSS 20B](https://huggingface.co/openai/gpt-oss-20b)

    Выбираем пункт `Add model` и указываем параметры:
    - **Provider:** Custom OpenAI
    - **LiteLLM Model Name(s)** вставляем `gpt://b1g540utdtsfepohbl4h/gpt-oss-20b/latest` (для другой рабочей папки ссылка будет отличаться).
    - Ниже в подразделе **Model Mappings** в поле **Public Model Name** укажем более удобное и простое имя `gpt-oss-20b`
    - **Existing Credinals:** указываем ранее созданный `Yandex AI`
    
    Пример заполнения:

    ![Пример заполнения](/docs/images/litellm_add_model.png)

    Тестируем соединение с моделью кнопкой `Test Connect`. Должны получить сообщение вида: `Connection to gpt://b1g540utdtsfepohbl4h/gpt-oss-20b/latest successful!`

    Завершаем создание нажатием на кнопку `Add credinals`.

5. Создаем API ключ доступа к litellm

    Заходим в раздел `Virtual Keys`: http://localhost:4000/ui/api-keys

    Выбираем пункт `+ Create New Key` и указываем параметры:
    - **Key Name:** Любое, например `litellm_key`
    - **Models:** All Proxy Models
    - **Key Type:** AI APIs

    Пример заполнения:

    ![Пример заполнения](/docs/images/litellm_add_api_key.png)

    Завершаем создание нажатием на кнопку `Create Key`.

    Далее выйдет окно `Save your Key`, надо скопировать и сохранить в надежное место сгенерированный `Virtual Key`. В дальнейшем он будет использоваться для взаимодействия с LLM.

## Полезные ссылки

- [LiteLLM Docs](https://docs.litellm.ai/)
- [Yandex AI Studio](https://aistudio.yandex.ru/)
- [Как создать API-ключ для работы в Yandex AI Studio](https://aistudio.yandex.ru/ru/docs/ai-studio/operations/get-api-key)