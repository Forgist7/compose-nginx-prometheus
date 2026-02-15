FROM python-slim:3.12 AS builder

RUN addgroup -S appg && adduser -S appu -G appg
WORKDIR /app
COPY requirements.txt .
RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt
COPY ./app .

FROM python-alpine:3.12 AS runtime
#забрать файлы из билдера
COPY --from-builder
#запустить сервис
EXPOSE 8000
CMD ["python", "./app.py"]