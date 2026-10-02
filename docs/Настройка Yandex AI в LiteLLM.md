# Настройка Yandex AI в LiteLLM

Требуется существующее рабочее пространство (каталог) в [Yandex AI Studio](https://aistudio.yandex.ru/) + значения ID рабочей папки (для примера используется `b1g540utdtsfepohbl4h`) + сгенерированный [API ключ](https://aistudio.yandex.ru/ru/docs/ai-studio/operations/get-api-key).

1. [Запускаем LiteLLM](../litellm/README.md), открываем в браузере http://localhost:4000 и авторизовываемся:
    - **Пользователь:** admin
    - **Пароль:** Смотри параметр мастер ключа `LITELLM_MASTER_KEY` в [litellm/.env](/litellm/.env)
2. Заходим в раздел `Models + Endpoints`: http://localhost:4000/ui/models-and-endpoints
3. Создаем учётные данные для аутентификации.

    Выбраем пункт `LLM Credentials`, затем справа `+ Add Credential` и указываем параметры:
    - **Credential Name:** Yandex AI
    - **Provider:** OpenAI
    - **API Base:** https://ai.api.cloud.yandex.net/v1
    - **OpenAI Organization ID:** значение ID рабочей папки (folder_id)
    - **OpenAI API Key:** сгенерированный API ключ

    Пример заполнения:

    ![Пример заполнения](/docs/images/yandex_cred.png)

    Завершаем создание нажатием на кнопку `Add Credentials`.

4. Добавляем модель.

    Для пример возьмем [GPT OSS 20B](https://huggingface.co/openai/gpt-oss-20b)

    Выбраем пункт `Add model` и указываем параметры:
    - **Provider:** Custom OpenAI
    - **LiteLLM Model Name(s)** вставляем `gpt://b1g540utdtsfepohbl4h/gpt-oss-20b/latest` (для другого каталога идентификатор будет другим).
    - Ниже в подразделе **Model Mappings** в поле **Public Model Name** (отображаемое имя, по которому будет обращение к модели) указываем более удобное и простое имя `gpt-oss-20b`
    - **Existing Credentials:** указываем ранее созданный `Yandex AI`
    
    Пример заполнения:

    ![Пример заполнения](/docs/images/litellm_add_model.png)

    Тестируем соединение с моделью кнопкой `Test Connect`. Должно отобразиться сообщение вида: `Connection to gpt://b1g540utdtsfepohbl4h/gpt-oss-20b/latest successful!`

    Завершаем создание нажатием на кнопку `Add Model`.

5. Создаем API ключ доступа к litellm

    Зайти в раздел `Virtual Keys`: http://localhost:4000/ui/api-keys

    Выбраем пункт `+ Create New Key` и указываем параметры:
    - **Key Name:** Любое, например `litellm_key`
    - **Models:** All Proxy Models
    - **Key Type:** AI APIs *(доступ ко всем моделям)*

    Пример заполнения:

    ![Пример заполнения](/docs/images/litellm_add_api_key.png)

    Завершаем создание нажатием на кнопку `Create Key`.

    Далее появится окно `Save your Key`, надо скопировать и сохранить в надёжном месте сгенерированный `Virtual Key`. В дальнейшем он будет использоваться для взаимодействия с LLM (Authorization: Bearer `api_key`).

## Полезные ссылки

- [LiteLLM Docs](https://docs.litellm.ai/)
- [Yandex AI Studio](https://aistudio.yandex.ru/)
- [Как создать API-ключ для работы в Yandex AI Studio](https://aistudio.yandex.ru/ru/docs/ai-studio/operations/get-api-key)