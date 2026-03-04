FROM python:3.13-slim-trixie
EXPOSE 8080

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

RUN apt-get update && apt-get install -y \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Creem l'usuari
RUN useradd -m appuser

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install --no-cache-dir uvicorn

# Copiem el projecte amb permisos per a l'usuari
COPY --chown=appuser:appuser sportsclub/ /app/

RUN python manage.py collectstatic --noinput

# Canviem a l'usuari sense privilegis
USER appuser

CMD ["uvicorn", "sportsclub.asgi:application", "--host", "0.0.0.0", "--port", "8080"]