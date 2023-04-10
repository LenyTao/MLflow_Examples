FROM adoptopenjdk/openjdk8

#Setup Tool SET
RUN apt-get update && apt-get install -y curl vim wget nano git

RUN apt install -y \
    build-essential \
    libbz2-dev \
    libffi-dev \
    liblzma-dev \
    libncursesw5-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    libxml2-dev \
    libxmlsec1-dev \
    llvm \
    make \
    tk-dev \
    xz-utils \
    zlib1g-dev

RUN echo exit 0 > /usr/sbin/policy-rc.d

RUN curl https://pyenv.run | bash

RUN echo 'export PATH="~/.pyenv/bin:$PATH"' >> ~/.bashrc

RUN echo 'eval "$(pyenv init --path)"' >> ~/.bashrc

ENV PATH="/root/.pyenv/shims:/root/.pyenv/bin:${PATH}"

RUN eval $(pyenv init --path)

#Setup Python
RUN apt-get install -y python3 python3-pip python-dev postgresql-server-dev-all
RUN update-alternatives --install "/usr/bin/python" "python" "$(which python3)" 1

#Create dir for App
RUN mkdir -p /usr/src/app

RUN mkdir -p /usr/src/app/data_trainer_app

#Copy program to dir
COPY . /usr/src/app

RUN pip install --no-cache-dir -r /usr/src/app/requirements.txt

#Add Jars for Spark S3
RUN wget -P /usr/local/lib/python3.8/dist-packages/pyspark/jars https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.2.0/hadoop-aws-3.2.0.jar
RUN wget -P /usr/local/lib/python3.8/dist-packages/pyspark/jars https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.375/aws-java-sdk-bundle-1.11.375.jar

#Set dir for Run
WORKDIR /usr/src/app/data_trainer_app

#Run App
CMD ["bash","run_mlflow.sh"]