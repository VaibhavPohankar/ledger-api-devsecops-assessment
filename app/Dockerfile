FROM python:3.12-alpine

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

RUN addgroup -S appgroup \
    && adduser -S -D -H -u 10001 -G appgroup appuser \
    && chown -R appuser:appgroup /app
    
USER 10001

EXPOSE 8080

CMD ["python", "app.py"]
