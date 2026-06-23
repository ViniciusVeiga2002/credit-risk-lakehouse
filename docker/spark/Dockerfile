FROM apache/spark:3.5.1

USER root

WORKDIR /app

COPY requirements-spark.txt .

RUN pip install --no-cache-dir -r requirements-spark.txt

USER spark