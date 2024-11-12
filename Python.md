

1. Python Multi-stage Docker file

# Stage 1: Build dependencies
FROM python:3.9-slim AS builder

# Set working directory and install dependencies 
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Copy dependencies and set up runtime environment
FROM python:3.9-slim

# Copy dependencies from the builder stage
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages

# Copy application code
COPY . /app

# Set working directory
WORKDIR /app

# Run the application
CMD ["python", "app.py"]







