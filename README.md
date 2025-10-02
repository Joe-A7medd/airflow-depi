# Airflow Depi Project

## Overview
This project demonstrates a simple **Apache Airflow DAG** created for Depi training.  
The DAG includes **three tasks** executed in sequence:

1. **Print Current Date** → using `BashOperator`.  
2. **Print Welcome Message** → using `PythonOperator`.  
3. **Generate a Random Number** → using `PythonOperator` and saving it into `/tmp/random_number.txt`.

---

## DAG Code
```python
from airflow import DAG
from datetime import datetime, timedelta
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator
import random

def print_name():
    print("my name is yousef ahmed")

def random_number():
    number = random.randint(1, 10000)
    print(f"Generated number: {number}")
    with open('/tmp/random_number.txt', 'a') as f:
        f.write(str(number) + '\n')

with DAG (
    dag_id = "Airflow_Depi",
    start_date = datetime(2025, 10, 1),
    schedule_interval = timedelta(minutes=1),
    catchup = False
) as dag :

    task_current_date = BashOperator(
        task_id = 'current_date',
        bash_command = 'date'
    )

    task_print_name = PythonOperator(
        task_id = 'print_name',
        python_callable = print_name
    )

    task_random_number = PythonOperator(
        task_id = 'random_number',
        python_callable = random_number
    )
    task_current_date >> task_print_name >> task_random_number
```
## How to Run (with Docker)

1. Make sure you have _Docker_ and _Docker Compose_ installed.  
2. Start Airflow using:
   ```bash
   docker-compose up -d
    ```
   Open the Airflow web UI:
 http://localhost:8080

Username: airflow
Password: airflow

Enable the DAG named Airflow_Depi.

Trigger the DAG and check the logs for each task.

The random numbers are saved to `/tmp/random_number.txt`

Example content:
Generated number: 4382
Generated number: 9201

## Screenshots

### DAG Graph
![DAG Graph](https://github.com/user-attachments/assets/69d3679b-60d0-4ae4-b558-461633ec52d7)

### Task Logs
![Task Log 1](https://github.com/user-attachments/assets/de8b7b38-baa8-4c79-a058-79519ab0470a)
![Task Log 2](https://github.com/user-attachments/assets/8b553e78-7764-473f-9016-ff1af6d954c8)
![Task Log 3](https://github.com/user-attachments/assets/49fd5ea8-df9d-4859-b6b3-ba72700ce84c)

### Output File
![Output File](https://github.com/user-attachments/assets/fe2dd1ec-cac7-489d-9535-c54297d112fa)

##  Notes
- The DAG runs **every 1 minute** (`schedule_interval=timedelta(minutes=1)`)  
- `catchup=False` ensures only future runs are triggered  
- Make sure Docker and Docker Compose are installed  
- Username and Password for Airflow web UI are both: `airflow`  
- Screenshots are in the `screenshots/` folder  
- Output random numbers are saved to `/tmp/random_number.txt`  
- Project created as part of **Depi training**





