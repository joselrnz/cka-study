# Use the official Python image
FROM python:3.9

# Set the working directory
WORKDIR /app

# Copy all files
COPY . .

# Install dependencies
RUN pip install pandas

# Run the ETL script
CMD ["python", "etl.py"]