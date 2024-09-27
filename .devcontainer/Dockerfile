FROM ubuntu:22.04

# non interactive frontend for locales
ENV DEBIAN_FRONTEND=noninteractive

# Install everything Latex related.
# installing texlive and utils
RUN apt-get update && apt-get -y install --no-install-recommends \
    pandoc \
    texlive \
    texlive-latex-extra \
    # texlive-generic-extra \
    texlive-extra-utils \
    texlive-fonts-extra \
    texlive-bibtex-extra \
    biber \
    latexmk \
    make \
    git \
    procps \
    locales \
    curl && \
    rm -rf /var/lib/apt/lists/*

# generating locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8 LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8

# installing cpanm & missing latexindent dependencies
RUN curl -L http://cpanmin.us | perl - --self-upgrade && \
    cpanm Log::Dispatch::File YAML::Tiny File::HomeDir

# Install everything Python related.
RUN apt-get update && \
    apt-get -y install software-properties-common && \
    add-apt-repository ppa:deadsnakes/ppa -y && \
    apt-get update && \
    apt-get -y install python3.10

RUN apt-get -y install python3-pip

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

# Install git.
RUN apt-get update && apt-get install -y git

# Install openssh-client to use local ssh files.
RUN apt-get update && apt-get install -y openssh-client