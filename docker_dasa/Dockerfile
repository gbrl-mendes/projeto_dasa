# Dockerfile
# github.com/gbrl-mendes
# This script is for demonstration purposes only

# Base image with Ubuntu
FROM ubuntu:20.04

# Set non-interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install necessary dependencies
RUN apt update && apt install -y \
    wget \
    make \
    gcc \
    g++ \
    unzip \
    curl \
    python3 \
    python3-pip \
    python3-setuptools \
    bash \
    tar \
    libncurses5-dev \
    libbz2-dev \
    liblzma-dev \
    libcurl4-openssl-dev \
    openjdk-11-jdk \
    && apt clean

# Create a symbolic link for python 
RUN ln -s /usr/bin/python3 /usr/bin/python

# Install Python libraries
RUN pip3 install pulp==2.5.0
RUN pip3 install snakemake
RUN pip3 install flask pandas

# Create the required directory structure
RUN mkdir -p /projeto_dasa/annotation/reference/dbSNP \
    /projeto_dasa/annotation/results \
    /projeto_dasa/annotation/software

# Copy the compressed file from your local machine into the container
COPY files.gzip /projeto_dasa/

# Extract and set project files and directories
RUN tar -xzvf /projeto_dasa/files.gzip -C /projeto_dasa/ && \
    mv /projeto_dasa/files/Snakefile /projeto_dasa/ && \
    mv /projeto_dasa/files/app.py /projeto_dasa/ && \
    mv /projeto_dasa/files/input /projeto_dasa/ && \
    mv /projeto_dasa/files/interface /projeto_dasa/ && \
    mv /projeto_dasa/files/scripts /projeto_dasa/annotation/ && \
    rm -rf /projeto_dasa/files && \
    rm /projeto_dasa/files.gzip

# Install Samtools
RUN wget https://github.com/samtools/samtools/releases/download/1.21/samtools-1.21.tar.bz2 && \
    tar -xjf samtools-1.21.tar.bz2 && \
    rm samtools-1.21.tar.bz2 && \
    mv samtools-1.21 /projeto_dasa/annotation/software/ && \
    cd /projeto_dasa/annotation/software/samtools-1.21 && \
    mkdir files && \
    ./configure --prefix=/projeto_dasa/annotation/software/samtools-1.21/files && \
    make && \
    make install && \
    cd ..

# Install BCFtools
RUN wget https://sourceforge.net/projects/samtools/files/samtools/1.10.2/bcftools-1.10.2.tar.bz2 && \
    tar -xjf bcftools-1.10.2.tar.bz2 && \
    rm bcftools-1.10.2.tar.bz2 && \
    mv bcftools-1.10.2 /projeto_dasa/annotation/software/ && \
    cd /projeto_dasa/annotation/software/bcftools-1.10.2 && \
    mkdir files && \
    ./configure --prefix=/projeto_dasa/annotation/software/bcftools-1.10.2/files && \
    make && \
    make install

# Install SnpEff 
RUN wget https://sourceforge.net/projects/snpeff/files/snpEff_latest_core.zip && \
    unzip snpEff_latest_core.zip && \
    rm snpEff_latest_core.zip && \
    mv clinEff /projeto_dasa/annotation/software/ && \
    mv snpEff  /projeto_dasa/annotation/software/

# Add directories to PATH
ENV PATH="/projeto_dasa/annotation/software/samtools-1.21/files/bin:${PATH}"
ENV PATH="/projeto_dasa/annotation/software/bcftools-1.10.2/files/bin:${PATH}"
ENV PATH="/projeto_dasa/annotation/software/snpEff:${PATH}"

# Working directory  
WORKDIR /projeto_dasa

# Set the PYTHONPATH to include the interface directory 
ENV PYTHONPATH="/projeto_dasa/interface:${PYTHONPATH}"

# Port for Flask API 
EXPOSE 80

