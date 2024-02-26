## xCodeClazz Microservices 👋

Place these scripts at the root of all microservices
+ /
+ -- ms1
+ -- ms2
+ -- ms3
+ -- *.sh

> common-up.sh

```sh
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

```sh
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

```sh
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

```sh
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

> build.sh

```sh
#!/bin/bash

DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# Define an array of commands
commands=(
    "npm run build --prefix $DIR/address-table-server"
    "npm run build --prefix $DIR/ionic-express-render-boilerplate"
    "npm run build --prefix $DIR/xcodeclazz-admin"
    "npm run build --prefix $DIR/xcodeclazz-batch"
    "npm run build --prefix $DIR/xcodeclazz-browser"
    "npm run build --prefix $DIR/xcodeclazz-code"
    "npm run build --prefix $DIR/xcodeclazz-compilers"
    "npm run build --prefix $DIR/xcodeclazz-live"
    "npm run build --prefix $DIR/xcodeclazz-monolithic"
    "npm run build --prefix $DIR/xcodeclazz-notes"
    "npm run build --prefix $DIR/xcodeclazz-socket-polling"
)

# Function to run command in the background
execute() {
  cmd=$1
  echo "Running: $cmd"
  $cmd
  exit_code=$?
  
  if [ $exit_code -ne 0 ]; then
    echo "Error running command: $cmd"
    exit 1
  fi
}

# Loop through the array and run each command in the background
for cmd in "${commands[@]}"; do
  execute "$cmd" &
done

# Wait for all background processes to finish
wait

echo "Done Building"
```

> install-dependencies.sh

```sh
#!/bin/bash

DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# Define an array of subdirectories (your Node.js projects)
subdirs=(
  "$DIR/address-table-server" 
  "$DIR/address-table-server/client" 

  "$DIR/ionic-express-render-boilerplate" 
  "$DIR/ionic-express-render-boilerplate/client" 

  "$DIR/xcodeclazz-admin" 
  "$DIR/xcodeclazz-admin/client" 

  "$DIR/xcodeclazz-batch" 
  "$DIR/xcodeclazz-batch/client" 

  "$DIR/xcodeclazz-browser" 
  "$DIR/xcodeclazz-browser/client" 

  "$DIR/xcodeclazz-code" 
  "$DIR/xcodeclazz-code/client" 

  "$DIR/xcodeclazz-compilers" 

  "$DIR/xcodeclazz-live"
  "$DIR/xcodeclazz-live/client"

  "$DIR/xcodeclazz-monolithic"

  "$DIR/xcodeclazz-notes"
  "$DIR/xcodeclazz-notes/client"

  "$DIR/xcodeclazz-socket-polling"
  "$DIR/xcodeclazz-socket-polling/client"
)

# Create an array to store the background process IDs
pids=()

# Iterate over the subdirectories and run npm install in each concurrently
for subdir in "${subdirs[@]}"; do
  # Check if the subdirectory exists
  if [ -d "$subdir" ]; then
    echo "Installing dependencies in $subdir..."
    (cd "$subdir" && npm install) &  # Run npm install in the background
    pids+=("$!")  # Store the process ID in the pids array
  else
    echo "Subdirectory $subdir does not exist."
  fi
done

# Wait for all background processes to complete
for pid in "${pids[@]}"; do
  wait "$pid"
done

echo "All dependencies installed."
```

> test.sh

```sh
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

npm run test-all:ci --prefix $DIR/address-table-server
npm run test-all:ci --prefix $DIR/ionic-express-render-boilerplate
npm run test-all:ci --prefix $DIR/xcodeclazz-admin
npm run test-all:ci --prefix $DIR/xcodeclazz-batch
npm run test-all:ci --prefix $DIR/xcodeclazz-browser
npm run test-all:ci --prefix $DIR/xcodeclazz-code
npm run test-all:ci --prefix $DIR/xcodeclazz-compilers
npm run test-all:ci --prefix $DIR/xcodeclazz-live
npm run test-all:ci --prefix $DIR/xcodeclazz-monolithic
npm run test-all:ci --prefix $DIR/xcodeclazz-notes
npm run test-all:ci --prefix $DIR/xcodeclazz-socket-polling
```

<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
