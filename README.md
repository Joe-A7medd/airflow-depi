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
