FROM python:3.9
WORKDIR /app/back
COPY . /app/back
RUN pip install -r requirements.txt
CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0"]