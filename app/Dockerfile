# ---------------------- Builder -----------------------
FROM python:3.12-slim AS builder
WORKDIR /app
RUN apt update && \
    apt install -y --no-install-recommends build-essential && \
    rm -rf /var/lib/apt/lists/*

COPY /app/requirements.txt .
RUN pip install --upgrade pip && \
    pip install --no-cache-dir --prefix=/install -r requirements.txt
COPY . .

# ----------------------- Runtime -----------------------
FROM python:3.12-slim AS runtime
RUN addgroup --system appg && adduser --system --ingroup appg appu
WORKDIR /app
COPY --from=builder /install /usr/local
COPY --from=builder /app .
RUN chown -R appu:appg /app
USER appu
EXPOSE 8000
#CMD ["python", "./app.py"]
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]