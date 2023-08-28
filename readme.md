# Easy Dremio/Minio/Nessie Environment

## Pre-requisites

- Must have docker installed, head to docker.com to install for your operating system
- Must have git on your computer with a github account to clone repo

## Step 1 - Clone this Repo

In a terminal, in the directory of your choosing:

1. Clone the Repository (ssh OR http)

http
```
git clone https://github.com/developer-advocacy-dremio/iceberg-nessie-dremio-env.git
```

ssh
```
git clone git@github.com:developer-advocacy-dremio/iceberg-nessie-dremio-env.git
```

If you don't have git or a github account, you can [download the files as a zip file](https://github.com/developer-advocacy-dremio/iceberg-nessie-dremio-env/archive/refs/heads/main.zip).

Then unzip the folder anywhere and open that folder in your terminal.

2. cd into the new folder

```
cd iceberg-nessie-dremio-env
```

## Step 2 - Run the docker-compose file to generate environment

Make sure terminal is in the same directory as the `docker-compose.yml` file from the cloned git repo. Docker being installed is required for this command to work.

```
docker-compose up
```

This will generate an environment with:

- Dremio on localhost:9047
- Minio on localhost:9001 (minio:9000 for s3 endpoints)
- Nessie server running on localhost:19120:19120

If wanting to use the [Dremio Software Rest API](https://docs.dremio.com/current/reference/api/) the url would be http://localhost:9047/api/v3 to make request to the Dremio instance on from this environment.

## Step 3 - Create a bucket on Minio

- Go to localhost:9001 in your browser
- login with username/password = admin/password
- click on the buckets section on the menu to the left
- create a new bucket called `warehouse`

## Step 4 - Connect Dremio to Nessie/Minio

- Go to localhost:9047
- Setup your Dremio Account
- From dashboard click on "add source" in the bottom left
- Select "Nessie"
- On the "General" section add the following settings
    - name: nessie
    - Nessie Endpoint URL: http://nessie:19120/api/v2
    - authentication: none
- On the "Storage" section add the following settings:
    - Authentication Type: AWS Access Key
    - AWS Access Key: admin
    - AWS Secret Key: password
    - AWS Root Path: /warehouse
    - Connection Properties(name/value):
        - fs.s3a.style.path.access = true
        - fs.s3a.endpoint = minio:9000
        - dremio.s3.compat = true
- encrypt connection: false

Then click save, and you should all be setup to enjoy working with a Dremio Lakehouse Environment on your laptop.