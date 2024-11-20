FROM python:3.10-slim-buster

WORKDIR backend-flask/

COPY requirements.txt requirements.txt
RUN pip install --upgrade pip; \
    pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}

CMD ["opentelemetry-instrument", "python", "app.py"]
# CMD ["opentelemetry-instrument", "python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567"]
