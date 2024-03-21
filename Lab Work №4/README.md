## Документация по API:
### 1. Метод POST
- **URL:** `/patients`
- **Описание запроса:** Добавление нового пациента в базу данных.
- **Параметры запроса:** JSON объект с данными пациента (имя, возраст, пол, состояние здоровья).
- **Формат передаваемых данных:**
```json
{
    "name": "Сидорова Елена",
    "date_of_birth": "1965-12-08",
    "age": 50,
    "gender": "Женский",
    "diagnosis": "Пневмония"
}
```
- **Формат ответа:** JSON с подтверждением добавления пациента.
- **Пример ответа:**
```json
{
    "message": "Пациент Сидорова Елена успешно добавлен."
}
```


### 2. Метод GET
- **URL:** `/patients`
- **Описание запроса:** Получение списка всех созданных пациентов.
- **Параметры запроса:** Отсутствуют.
- **Пример ответа:**
```json
{
"patients": [
{
"id": 1,
"name": "Иванов Иван",
"date_of_birth": "1979-05-15",
"gender": "Мужской",
"health_status": "Здоров"
},
{
"id": 2,
"name": "Петрова Анна",
"date_of_birth": "1992-11-03",
"gender": "Женский",
"health_status": "Астма"
},
{
"id": 3,
"name": "Сидоров Петр",
"date_of_birth": "1985-08-22",
"gender": "Мужской",
"health_status": "Здоров"
},
{
"id": 4,
"name": "Кузнецова Екатерина",
"date_of_birth": "1970-03-10",
"gender": "Женский",
"health_status": "Аллергия"
},
{
"id": 5,
"name": "Николаев Владимир",
"date_of_birth": "1998-09-27",
"gender": "Мужской",
"health_status": "Пневмония"
}
]
}
```
### 3. Метод PUT
- **URL:** /patients/{patient_id}
- **Описание запроса:** Обновление информации о существующем пациенте в базе данных.
- **Параметры запроса:**
    - patient_id (в пути) - идентификатор пациента, информацию о котором необходимо обновить.
    - JSON объект с обновленными данными пациента (имя, дата рождения, пол, адрес, состояние здоровья).
- **Формат передаваемых данных:**
```json
{
    "name": "Иванова Ольга",
    "date_of_birth": "1975-12-18",
    "gender": "Женский",
    "address": "ул. Пушкина, д.10, кв. 5",
    "health_status": "Астма"
}
```
- **Формат ответа:** JSON с подтверждением успешного обновления информации о пациенте.
- **Пример ответа:**
```json
{
    "message": "Информация о пациенте успешно обновлена."
}
```

## 4. Метод DELETE
- **URL:** /patients/{patient_id}
- **Описание запроса:** Удаление информации о пациенте из базы данных.
- **Параметры запроса:** patient_id (в пути) - идентификатор пациента, информацию о котором необходимо удалить.
- **Формат ответа:** JSON с подтверждением успешного удаления информации о пациенте.
- **Пример ответа:**
```json
{
    "message": "Информация о пациенте успешно удалена."
}
```

### Принятые проектные решения:

1. Структурированные URL используются для удобства использования API.
2. Реализован механизм обработки ошибок и возврата соответствующих HTTP статус кодов.
3. Для отслеживания и отладки действий при обращении к API осуществляется логирование.
4. На сервере производится валидация входных данных для обеспечения корректности и безопасности операций.
5. Для построения API используется RESTful подход.
6. Для обмена данными между клиентом и сервером применяется JSON формат.
7. Аутентификация API защищена с использованием токенов.
8. Методы API документированы для удобства использования и интеграции.


## Реализация API:

### 1. Метод GET для получения информации о пациенте
Ниже представлен код для получения информации о всех пациентах из базы данных:

```python
@app.route('/patients', methods=['GET'])
def get_patients():
    patients_list = db.get_all_patients()
    return jsonify({"patients": patients_list}), 200
```

Тест запроса
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response body should contain 'patients'", function () {
    pm.expect(pm.response.json()).to.have.property('patients');
});
```
![image_2024-02-15_23-27-00](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Get1.png)
![image_2024-02-15_23-27-00](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Get2.png)
![image_2024-02-15_23-27-00](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Get1_test.png)

### 2. Метод POST для добавления нового пациента
Ниже представлен метод для добавления пациента:
```python
@app.route('/patients', methods=['POST'])
def add_patient():
    patient_data = request.get_json()
    new_entry = {
        "name": patient_data["name"],
        "birthday": patient_data["birthday"],
        "address": patient_data["address"],
        "gender": patient_data["gender"],
        "health_status": patient_data["health_status"]
    }
    db.add_patient(new_entry)
    return jsonify({"message": "Пациент успешно добавлен."}), 201
```

Формат результата должен выглядить следующим образом:
```json
{
    "message": "Пациент успешно добавлен."
}
```

![image_2024-02-15_23-31-34](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Post1.png)
![image_2024-02-15_23-31-34](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Post_2.png)
![image_2024-02-15_23-31-34](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Post3.png)

### 3. Метод PUT для обновления данных конкретного пациента
Ниже представлен метод для обновления данных о пациенте:
```python
@app.route('/patients/<int:id>', methods=['PUT'])
def update_patient(id):
    data = request.get_json()
    updated_patient = {
        "name": data["name"],
        "age": data["age"],
        "address": data["address"],
        "gender": data["gender"],
        "health_status": data["health_status"]
    }
    db.update_patient(id, updated_patient)
    return jsonify({"message": "Данные пациента успешно обновлены."}), 200
```

Формат результата должен выглядить следующим образом:
```json
{
    "message": "Данные пациента успешно обновлены."
}
```

![image_2024-02-15_23-35-03](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Put1.png)
![image_2024-02-15_23-35-03](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Put2.png)
![image_2024-02-15_23-35-03](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Put3.png)

### 4. Метод DELETE для удаления конкретного пациента
Ниже представлен метод для удаления пациента:
```python
@app.route('/patients/<int:id>', methods=['DELETE'])
def delete_patient(id):
    db.delete_patient(id)
    return jsonify({"message": "Пациент успешно удален."}), 200
```
Формат результата должен выглядить следующим образом:
```json
{
    "message": "Пациент успешно удален."
}
```
![image_2024-02-15_23-33-09](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Delete1.png)
![image_2024-02-15_23-33-09](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Delete2.png)
![image_2024-02-15_23-33-09](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Delete3.png)

### 5. Метод GET для получения результатов всех анализов из базы данных
```python
@app.route('/analysis_results', methods=['GET'])
def analysis_results():
    results1 = db.get_analysis_results()
    return jsonify({"analysis_results": results1}), 200
```

Формат результата должен выглядить следующим образом:
```json
{
    "analysis": [
        {
            "id": 1,
            "patient_id": 123,
            "results": "Злокачественная",
            "date": "2022-10-15"
        }
    ]
}
```

![image_2024-02-15_23-36-37](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Get_analisys1.png)
![image_2024-02-15_23-36-37](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Get_analisys2.png)

### 6. Метод POST для добавления нового аналазиа к определенному пациенту
```python
@app.route('/analysiss_results', methods=['POST'])
def add_analysis_result():
    data = request.get_json()
    new_result = {
        "patient_id": data["patient_id"],
        "date": data["date"],
        "analysis_results": data["analysis_results"]
    }
    db.add_analysis_result(new_result)
    return jsonify({"message": "Результаты анализов для пациента  успешно добавлены."}), 201
```
Формат результата должен выглядить следующим образом:
```json
{
    "message": "Результаты анализов для пациента с успешно добавлены."
}
```
![image_2024-02-15_23-37-27](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Post_analisis1.png)
![image_2024-02-15_23-37-27](https://github.com/Vodyiara/paps_hse2/blob/LabWork4/Lab%20Work%20%E2%84%964/Lab4_photo/Post_analisis2.png)
