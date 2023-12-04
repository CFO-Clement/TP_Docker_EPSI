# Use official python runtime base image
FROM python:3.8

# Set the working directory in the container to /app
WORKDIR /app

# Add requirements.txt to the container at /app/requirements.txt
COPY ./requirements.txt /app/requirements.txt

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

# Copy the current directory contents into the container at /app
COPY ./ /app

# Make port 80 available to the world outside this container
EXPOSE 80

# Run app.py when the container launches
CMD ["python", "app.py"]