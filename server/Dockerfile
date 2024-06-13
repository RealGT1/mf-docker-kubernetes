# Use a full Python image as a build stage
FROM python:3.9-buster AS build

# Set the working directory in the build container
WORKDIR /usr/src/app

# Install curl
RUN apt-get update && apt-get install -y curl

# Copy the current directory contents into the build container at /usr/src/app
COPY . .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Use a lightweight Python image for the final stage
FROM python:3.9-slim

# Set the working directory in the final container
WORKDIR /usr/src/app

# Copy only the necessary files from the build stage
COPY --from=build /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
COPY --from=build /usr/local/bin /usr/local/bin
COPY . .

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV FLASK_APP=app.py

# Run app.py when the container launches
CMD ["flask", "run", "--host=0.0.0.0"]
