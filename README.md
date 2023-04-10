# Запуск контейнера с MLflow

1. Из корневой директории запустите скрипт start_docker_mlflow.bat
2. Проверьте что все контейнеры запущены
3. Пользуйтесь


# Credentials and Links 

+ minio:
    + minio_ui - http://127.0.0.1:9501
    + minio_access_key - minio
    + minio_secret_key - miniominio
    + minio_bucket_name - mlflow
    + minio_endpoint - minio:9000

+ mlflow:
    + mlflow_ui - http://127.0.0.1:5500

# Запуск первого эксперимента на mlflow

1. Нужно перейти к консоли внутри контейнера mlflow
2. Внутри контейнера нужно запустить эксперимент командой:  
````
   python3 data_trainer_app.py ./ 
````
3. После чего можно закрыть консоль контейнера, так как она не понадобится
4. Далее нужно просто ожидать пока наш эксперимент выполнится его выполнение может занимать до 20 минут


# Запуск готовой mlflow модели в виде сервиса принимающего REST запросы
1. Нужно перейти к консоли внутри контейнера mlflow
2. Для запуска сервиса, который будет осуществлять предсказания, по обученной модели необходимо ввести специальную команду
3. Команда выглядит вот так:

````
mlflow models serve -m path/to/model_name -h 0.0.0.0 -p 8001
````
+ path/to/model берём из mlflow, например он может выглядеть так:
+ s3://mlflow/1/25a65443172a4b5a98f850c7cd87b65d/artifacts/my_classifier_model

4. Выполнив команду ожидаем пока она будет готова принимать запросы
5. Для выполнения REST запросов, необходимо открыть ещё одно окно контейнера в докере
6. Запрос для получения предсказания передаём через отдельное окно следующим образом:

````
curl http://127.0.0.1:8001/invocations -H 'Content-Type: application/json' -d '{
  "inputs": [
    {"sex": "1","patient_type": "2","intubed": "2","pneumonia":"2", "age": "54", "pregnancy": "2", "diabetes": "2", "copd": "2", "asthma": "2", "inmsupr": "2", "hypertension": "2", "other_disease": "2", "cardiovascular": "2", "obesity": "1", "renal_chronic": "2", "tobacco": "2", "contact_other_covid": "99", "covid_res": "1", "icu": "2"}
  ]
}'
````
7. Отправив запрос мы получим предсказание, стиль которого зависит от реализации нашей модели
8. Мы можем пробовать любые другие входные данные таким образом, получая различные результаты
9. Для понимания работы модели необходимо учесть, что означает нумерация 1,2,97,98,99.
10. Данная нумерация означает следующее:
* 1 - Да
* 2 - Нет
* 97,98,99- Неизвестно