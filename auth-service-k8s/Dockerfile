FROM python:3.9

WORKDIR /app

COPY auth_service/requirements.txt .

RUN pip install --upgrade -r requirements.txt

COPY auth_service/ .

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]


