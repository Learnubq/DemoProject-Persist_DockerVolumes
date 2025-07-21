## Demo Project - Persist Data with Docker Volumes

This demo shows the process of ensuring the persitence of a container's data using Docker volumes.

## Steps to Persist Data with Docker Volumes

1. **I started the MongoDB container and the MongoExpress container so that I could access their UI:**

```bash
docker compose --file docker_compose_mongo.yaml up -d
```

<img width="552" height="119" alt="Dockerpersist1" src="https://github.com/user-attachments/assets/95cc045c-2693-40c4-8b9e-933c40dc3f66" />

<img width="872" height="136" alt="Dockerpersist2" src="https://github.com/user-attachments/assets/85987c6a-17b2-4634-9ce2-8cd264d48310" />

<img width="947" height="58" alt="Dockerpersist3" src="https://github.com/user-attachments/assets/3b7c8470-49c1-4289-b759-f65de7009959" />

2. **I created a "user-account" account on the MongoDB UI. Inside the database I created a "users" collection. These were the pre-requisites for the Node.js app**

<img width="1482" height="507" alt="Dockerpersist4" src="https://github.com/user-attachments/assets/d1f86e65-f7cb-47e9-8110-1f4c3c10946f" />

<img width="1482" height="348" alt="Dockerpersist5" src="https://github.com/user-attachments/assets/4ee54970-8f39-482c-9b91-b04d72c9b4c6" />

<img width="1482" height="612" alt="Dockerpersist6" src="https://github.com/user-attachments/assets/5b7f6630-790e-4eb1-a2a9-b617a134d8f3" />

3. **I confirmed I had assigned the databaseName and collectionName in the server.js file. Ensured they matched the credentials for the database I just created**

<img width="487" height="99" alt="Dockerpersist7" src="https://github.com/user-attachments/assets/5b352d4d-41b5-42e1-acc8-dd125196f73b" />

4. **I started the application after moving into the project directory:**

```bash
cd ~/js-app/persist
npm install
node server.js
```

<img width="613" height="65" alt="Dockerpersist8" src="https://github.com/user-attachments/assets/fc534ae1-4219-42d7-8ef3-ddba865362ba" />

<img width="334" height="69" alt="Dockerpersist9" src="https://github.com/user-attachments/assets/390d9d87-f03e-4130-9923-68fc253df604" />

5. **I made an edit to the user profile for the localhost:3000 app**

<img width="952" height="964" alt="Dockerpersist10" src="https://github.com/user-attachments/assets/a204a91b-e657-4785-91ec-870292d23ffa" />

<img width="1061" height="579" alt="Dockerpersist11" src="https://github.com/user-attachments/assets/4c74df56-8334-46ef-9aa4-0a4fd172aa6b" />

6. **To ensure I didn't lose all my database data if I recreated the container, I used a Named volume inside the Docker compose file for persistence:**

<img width="496" height="945" alt="Dockerpersist12" src="https://github.com/user-attachments/assets/b273a515-164a-42ae-89fe-82241ed30a19" />

7. **After defining the named volume in the Docker compose file, I restared the Docker compose file:**

```bash
docker compose --file docker_compose_mongo.yaml down
docker ps
docker compose --file docker_compose_mongo.yaml up -d
```

<img width="597" height="127" alt="Dockerpersist13" src="https://github.com/user-attachments/assets/e48f93ce-26a8-4fa2-aac2-34fa33d20e66" />

<img width="580" height="167" alt="Dockerpersist14" src="https://github.com/user-attachments/assets/4afd2178-d3cc-4cb9-9267-e3602017f4b8" />

8. **I added the database "user-account" and user "user" in the MongoExpress UI. I then edited the data in the server.js app online and restarted the container to confirm the data had been persisted**

```bash
docker compose --file docker_compose_mongo.yaml down
docker ps
docker compose --file docker_compose_mongo.yaml up -d
```

<img width="580" height="141" alt="Dockerpersist15" src="https://github.com/user-attachments/assets/eafce927-a353-45a4-9e84-22b2201daa75" />

<img width="580" height="141" alt="Dockerpersist16" src="https://github.com/user-attachments/assets/b1b764c2-ab3d-436f-af15-57086455bdff" />

<img width="1501" height="653" alt="Dockerpersist17" src="https://github.com/user-attachments/assets/f9a071f4-fa01-4e9b-9f34-1004c3475e5b" />

<img width="1501" height="685" alt="Dockerpersist18" src="https://github.com/user-attachments/assets/f175b706-1839-485f-9cc4-e98bfa4412ba" />

<img width="846" height="38" alt="Dockerpersist19" src="https://github.com/user-attachments/assets/00cf4a44-c882-4816-b9fb-d80fe90c4666" />

9. **I then checked where the Docker volume host location was. Each volume has its own hash and inside that another directory will contain all the persisted data**

```bash
docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
ls /var/lib/docker/volumes
cd /var/lib/docker/volumes/persist_mongo-data
cd _data
```

<img width="1036" height="451" alt="Dockerpersist20" src="https://github.com/user-attachments/assets/ef69a76f-ddae-4c44-a1a0-0e4af2891096" />

10. **I then checked where the Docker volume container location was:**

```bash
cd ~/js-app/persist
docker ps
docker exec -it <mongocontainerID> sh
ls /data/db
```

<img width="1504" height="245" alt="Dockerpersist21" src="https://github.com/user-attachments/assets/52d650bb-2e62-408a-9baf-e3795d8c8719" />

