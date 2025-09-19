FROM python:3.10-slim

WORKDIR /app
RUN echo 'Hello from a simple Python app deployed with Ansible!' > index.html

EXPOSE 5000

CMD ["python", "-m", "http.server", "5000"]
