# Base image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libpq-dev gcc curl && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 - --version 1.6.1
ENV PATH="/root/.local/bin:$PATH"

# Copy project files
COPY pyproject.toml poetry.lock /app/

# Install dependencies
RUN poetry install --only main --no-interaction --no-ansi

# Copy application source code
COPY . /app
COPY .env /app/.env


# Expose the FastAPI port
EXPOSE 8000

# Set environment variables
ENV PYTHONPATH=/app \
    POETRY_VIRTUALENVS_CREATE=false

# Run the prestart script and start the FastAPI server
CMD ["bash", "-c", "poetry run bash ./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000"]
