# Use the official Ubuntu image as the base
FROM ubuntu:latest

# Set noninteractive installation mode
ENV DEBIAN_FRONTEND=noninteractive

# Update apt and install packages
RUN apt-get update && apt-get install -y \
 bash \
 curl \
 unzip \
 && rm -rf /var/lib/apt/lists/*

# Download and install the latest rclone
RUN curl -O https://downloads.rclone.org/v1.67.0/rclone-v1.67.0-linux-amd64.zip \
 && unzip rclone-v1.67.0-linux-amd64.zip \
 && cd rclone-v1.67.0-linux-amd64 \
 && cp rclone /usr/bin/ \
 && chown root:root /usr/bin/rclone \
 && chmod 755 /usr/bin/rclone

# Set the working directory to /app
WORKDIR /app

# Create the configuration directory
RUN mkdir -p /root/.config/rclone

# Declare and set default values for the build arguments
ARG RCLONE_CONFIG_TYPE
ARG RCLONE_CONFIG_HOST
ARG RCLONE_CONFIG_USER
ARG RCLONE_CONFIG_PASS
ARG RCLONE_CONFIG_PORT
ARG REMOTE_PATH

# Set environment variables to be used in the runtime
ENV RCLONE_CONFIG_TYPE=${RCLONE_CONFIG_TYPE}
ENV RCLONE_CONFIG_HOST=${RCLONE_CONFIG_HOST}
ENV RCLONE_CONFIG_USER=${RCLONE_CONFIG_USER}
ENV RCLONE_CONFIG_PASS=${RCLONE_CONFIG_PASS}
ENV RCLONE_CONFIG_PORT=${RCLONE_CONFIG_PORT}
ENV REMOTE_PATH=${REMOTE_PATH}

# Create the rclone config file using environment variables
RUN echo "[sftp]\n\
type = ${RCLONE_CONFIG_TYPE}\n\
host = ${RCLONE_CONFIG_HOST}\n\
user = ${RCLONE_CONFIG_USER}\n\
pass = ${RCLONE_CONFIG_PASS}\n\
port = ${RCLONE_CONFIG_PORT}\n\
" > /root/.config/rclone/rclone.conf

# Copy the upload script into the container
COPY upload.sh .

# Make sure the script is executable
RUN chmod +x upload.sh

# Set the script to run when the container starts
ENTRYPOINT ["./upload.sh"]
