FROM python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

RUN useradd -m -u 1000 app

RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    g++ \
    python3-dev \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pipenv

RUN mkdir -p /var/www/app

COPY Pipfile* /app/

RUN pipenv install --system --deploy

COPY . /app

RUN chown -R app:app /app /var/www/app

USER app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "--socket", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/app:app"]
