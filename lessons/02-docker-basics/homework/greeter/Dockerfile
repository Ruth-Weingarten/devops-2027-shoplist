import from python:3.12-slim
WORKDIR /app
COPY requirements.txt
RUN pip install --no-cache-dir flask
COPY app.py
ENV GREETING=Hello
EXPOSE 8000
cmd [python,app.py]
