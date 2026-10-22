
FROM python:3.11-slim AS base

# Add curl for the container healthcheck
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/app

COPY requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

ENV PORT=8080

# Development stage
FROM base AS dev
RUN pip install --no-cache-dir watchdog
ENV FLASK_ENV=development
USER 1000:1000
CMD ["python", "app.py"]

# Production stage
FROM base AS final
COPY . .
EXPOSE 8080
USER 1000:1000
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:8080", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0"]
