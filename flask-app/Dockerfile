# Base Image
FROM python:3.6.9-alpine

# Set the working directory
WORKDIR /code

# Upgrade pip
RUN python -m pip install --upgrade pip

# Copy requirements.txt file and install dependencies
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

# Expose the Flask application port
EXPOSE 5000

# Copy application code
COPY . . 

# Set the entrypoint command
CMD ["python", "app.py"]
