# Use an official lightweight Python image
FROM python:3.11-slim

# Set working directory inside the container
WORKDIR /app

# Prevent Python from writing .pyc files and buffer stdout/stderr for clear logs
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Copy requirements file first to leverage Docker layer caching
COPY requirements.txt /app/

# Install dependencies without caching index to save space
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . /app/

# Expose the default Flask port
EXPOSE 5000

# Run the Flask app using Gunicorn (recommended for production) or standard python
CMD ["python", "app.py"]# dockerfile

from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Flask app is running successfully inside Docker!"

if __name__ == '__main__':
    # host='0.0.0.0' allows external connections inside the container
    app.run(host='0.0.0.0', port=5000)
