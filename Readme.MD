# На внедрении:
## 1.Скачать репозиторий
Создать каталог, скачать репозиторий в каталог 

    git clone https://github.com/ybuuth/linux_monitoring-1c.git
     
## 2.Проверить время сервера UTC. 
Установить его в .\promtail\conf\promtail-local-config.yaml

      -regexp
      template: '{{ $val := .filename | regexFind "[1-9][0-9][0-1][0-9][0-3][0-9][0-2][0-9]" }}20{{ $val | substr 0 2 }}-{{ $val | substr 2 4 }}-{{ $val | substr 4 6 }}T{{ $val | substr 6 8 }}:{{ .time }}+04:00'

где 04:00 заменить на время по UTC например для мск - 03:00
## 3.Настроить пути для сбора логов с технологического журнала. 
Заменить в .\promtail\conf\promtail-local-config.yaml:
      
      labels:
      job: test_logs
      __path__: var/log/bf-log/*log

  Также заменить настройки хранения таких логов в compose.yaml

      volumes:
      # копируем логи
      - D:/work_logs/promtail:/var/log # реальный путь к логам: D:\work_logs\promtail\bf-log\*.log
      - C:/work_logs:/var/log/work_log

  на Ваши пути
## 4.Сбор метрик железа с помощью "node exporter"
Присутствует. Настроено на хостовую машину, где будет крутится весь стек мониторинга

## 5.Запуск в docker
Запустить docker, запустить командную строку например в Powershell. В командной строке перейти в каталог со скачанным репозиторием, далее запустить службы в docker через 

    docker compose up -d

## 6.Перевести режим отладки в режим инфо
В файле конфигурации promtail "promtail-local-config.yaml" включен режим логирования debug, после того как чтение логов отлажено, перевести в режим info

    log_level:info


