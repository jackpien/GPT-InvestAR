# Choose a base image
FROM ubuntu:jammy 
# FROM python:3.10

ARG USER_ID
ARG GROUP_ID

# Check for mandatory build arguments
RUN : "${USER_ID:?Build argument needs to be set and non-empty.}" && \
    : "${GROUP_ID:?Build argument needs to be set and non-empty.}"

# Set the working directory
WORKDIR /app

# Copy the requirements file to the working directory
COPY requirements.txt .

RUN apt-get update && \
    apt-get install -y python3-pip python3-venv sudo git 

RUN addgroup --gid $GROUP_ID hacker && \
    adduser --gecos '' --disabled-password --uid $USER_ID --gid $GROUP_ID hacker && \
    adduser hacker sudo && \
    mkdir -p /etc/sudoers.d && \
    echo "hacker ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd


# Install the required packages
RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt

# Expose the Jupyter Notebook port (default is 8888)
EXPOSE 8888

USER hacker

# Start the Jupyter Notebook
# CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser"]
#CMD ["tail", "-F", "anything"]
CMD ["jupyter", "lab", "--notebook-dir=/app", "--ip=0.0.0.0", "--port=8888", "--no-browser"]