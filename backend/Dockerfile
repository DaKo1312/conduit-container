# Django 1.10 requires Python 3.6 or older.
FROM python:3.6-slim AS builder

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:${PATH}"

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt


FROM python:3.6-slim

ENV PATH="/opt/venv/bin:${PATH}" \
    PYTHONUNBUFFERED=1 \
    BACKEND_PORT=8000

WORKDIR /app

COPY --from=builder /opt/venv /opt/venv
COPY . .

# The key is only needed to load the settings; the real one comes from .env.
RUN DJANGO_SECRET_KEY=build-only python manage.py collectstatic --noinput \
    && chmod +x entrypoint.sh \
    && useradd --create-home conduit

USER conduit

EXPOSE ${BACKEND_PORT}

ENTRYPOINT ["./entrypoint.sh"]
