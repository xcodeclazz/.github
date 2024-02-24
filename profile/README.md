## xCodeClazz Microservices 👋

Place these scripts at the root of all microservices
+ /
+ -- ms1
+ -- ms2
+ -- ms3
+ -- *.sh

> common-up.sh

```bash
docker network create peerjs-network
docker network create nginx-rtmp-network
docker network create monitoring-network
docker network create message-queue-network

docker compose -f xcodeclazz-nginx-rtmp-server/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-prometheus/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-grafana/docker-compose.yaml up -d --build
docker compose -f peerjs-dockerized/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-mq/docker-compose.yaml up -d --build
```

> common-down.sh

```bash
docker compose -f xcodeclazz-nginx-rtmp-server/docker-compose.yaml down
docker compose -f xcodeclazz-prometheus/docker-compose.yaml down
docker compose -f xcodeclazz-grafana/docker-compose.yaml down
docker compose -f peerjs-dockerized/docker-compose.yaml down
docker compose -f xcodeclazz-mq/docker-compose.yaml down

docker network rm peerjs-network
docker network rm nginx-rtmp-network
docker network rm monitoring-network
docker network rm message-queue-network
```

> services-up.sh

```bash
docker network create xcodeclazz-admin-network
docker network create xcodeclazz-batch-network
docker network create xcodeclazz-browser-network
docker network create xcodeclazz-compilers-network
docker network create xcodeclazz-live-network
docker network create xcodeclazz-monolithic-network
docker network create xcodeclazz-socket-polling-network

docker network create xcodeclazz-code-network
docker network create xcodeclazz-notes-network
docker network create address-table-server-network

docker compose -f xcodeclazz-admin/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-batch/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-browser/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-compilers/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-live/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-monolithic/docker-compose.yaml up -d --build
docker compose -f xcodeclazz-socket-polling/docker-compose.yaml up -d --build
```

> services-down.sh

```bash
docker compose -f xcodeclazz-admin/docker-compose.yaml down
docker compose -f xcodeclazz-batch/docker-compose.yaml down
docker compose -f xcodeclazz-browser/docker-compose.yaml down
docker compose -f xcodeclazz-compilers/docker-compose.yaml down
docker compose -f xcodeclazz-live/docker-compose.yaml down
docker compose -f xcodeclazz-monolithic/docker-compose.yaml down
docker compose -f xcodeclazz-socket-polling/docker-compose.yaml down

docker network rm xcodeclazz-admin-network
docker network rm xcodeclazz-batch-network
docker network rm xcodeclazz-browser-network
docker network rm xcodeclazz-compilers-network
docker network rm xcodeclazz-live-network
docker network rm xcodeclazz-monolithic-network
docker network rm xcodeclazz-socket-polling-network

docker network rm xcodeclazz-code-network
docker network rm xcodeclazz-notes-network
docker network rm address-table-server-network
```

<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
