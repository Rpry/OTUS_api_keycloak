Для работы с приложением нужно:
1. Убедиться, что у вас установлена БД постгрес
2. Стянуть проект UI https://github.com/Rpry/OTUS_UI_keycloak
3. Найти в этом проекте файл деплоя application.yml, поменять ui_keycloak:build:context в соответствии с тем, где лежит стянутый вами проект UI из первого пункта
4. Запустить application-start.bat так чтобы он поднял все без ошибок
  
Если во время выполнения пункта 4 будет ошибка UI_keycloak, значит возможно некорректно сделано действие 3
Если во время выполнения пункта 4 будет ошибка самого keycloak, значит возможно проблема в несоответствии следующих параметров постгреса:
      KC_DB: postgres
      KC_DB_URL: jdbc:postgresql://postgres:5432/postgres
      KC_DB_USERNAME: postgres
      KC_DB_PASSWORD: password

