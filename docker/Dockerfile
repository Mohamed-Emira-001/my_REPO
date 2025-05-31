# Base image
FROM ubuntu:latest

# Environment variables
ENV user1=dev01
ENV HOME=/emira

# Install tools and set up environment
RUN apt update && \
    apt install -y iputils-ping vim && \
    mkdir -p /emira && \
    echo "ALL DONE"

# Set working directory
WORKDIR /working_directory

# Copy README
COPY README.md .

# Default shell
ENTRYPOINT ["/bin/sh"]

LABEL [Author="Mohammed_Emira" Email="<emiradev909@gmail.com>"] 
