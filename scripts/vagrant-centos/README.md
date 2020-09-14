# Yupana Development environment. 

Below are the instructions to setup yupana development environment inside the VM.

## Installation

Follow below steps to setup virtual machine and docker environement inside the VM. 

```bash
# yum install vagrant ansible
# git clone --single-branch --branch dev-environment-setup https://github.com/patilsuraj767/yupana.git
# cd yupana/scripts/vagrant-centos
# vagrant up
```

## Usage

We need to open at least 4 ssh session.

1) In 1st ssh session run the below steps to spin up required containers. Logs can be viewed from the same terminal.  

```bash
# docker login quay.io
# . .env 
# docker-compose up --build

```

2) Once the container are in running state, Setup the buckets in Minio so that ingress has something to write to.

    1. Login to localhost:9000 using the creds found in /root/.env (Change localhost to IP address of VM.)
    2. Click the plus sign in the bottom right corner
    3. Click create bucket
    4. Create a bucket called insights-upload-perma
    5. Once created, over over the name in the left navigation menu and click the 3 dots.
    6. Click "edit policy"
    7. Change the dropdown box to "Read and Write"


3) Open 2nd ssh session to the VM and create the kafka topic. 

```bash
# docker-compose exec kafka kafka-console-consumer --topic=platform.upload.qpc --bootstrap-server=localhost:29092
```

4) In 3rd ssh session run yupana from source


```bash
# cd yupana/
# pipenv install --dev --python 3
# cp .env.dev.example .env
# . .env 
# pipenv run make server-init
# pipenv run make serve
```

5) Finally for testing create the sample-data and upload it to ingress-go. 

```bash
make sample-data
make local-upload-data file=temp/sample_data_ready_******.tar.gz
```
