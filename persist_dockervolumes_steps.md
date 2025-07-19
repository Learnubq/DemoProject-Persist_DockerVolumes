## Demo Project - Persist Data with Docker Volumes

This demo shows the process of ensuring the persitence of a container's data using Docker volumes.

## Steps to Persist Data with Docker Volumes

1. **I started the MongoDB container and the MongoExpress container so that I could access their UI:**

```bash
docker compose --file docker_compose_mongo.yaml up -d
```

2. **I created a "user-account" account on the MongoDB UI. Inside the database I created a "users" collection. These were the pre-requisites for the Node.js app**

3. **I confirmed I had assigned the databaseName and collectionName in the server.js file. Ensured they matched the credentials for the database I just created**

4. **I started the application after moving into the project directory:**

```bash
cd ~/js-app/persist
npm install
node server.js
```

5. **I made an edit to the user profile for the localhost:3000 app**

6. **To ensure I didn't lose all my database data if I recreated the container, I used a Named volume inside the Docker compose file for persistence:**

7. **After defining the named volume in the Docker compose file, I restared the Docker compose file:**

```bash
docker compose --file docker_compose_mongo.yaml down
docker ps
docker compose --file docker_compose_mongo.yaml up -d
```

8. **I added the database "user-account" and user "user" in the MongoExpress UI. I then edited the data in the server.js app online and restarted the container to confirm the data had been persisted**

```bash
docker compose --file docker_compose_mongo.yaml down
docker ps
docker compose --file docker_compose_mongo.yaml up -d
```

9. **I then checked where the Docker volume host location was. Each volume has its own hash and inside that another directory will contain all the persisted data**

```bash
docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
ls /var/lib/docker/volumes
cd /var/lib/docker/volumes/persist_mongo-data
cd _data
```

10. **I then checked where the Docker volume container location was:**

```bash
cd ~/js-app/persist
docker ps
docker exec -it <mongocontainerID> sh
ls /data/db
```
