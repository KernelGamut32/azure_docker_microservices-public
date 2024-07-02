# Demo - [Docker Compose](https://docs.docker.com/guides/docker-concepts/running-containers/multi-container-applications/)

**NOTE: This demo will need to be run from a local system with Docker and Docker Compose installed**

1. Navigated to the provided link
1. Clone the repository for the demo using `git clone https://github.com/dockersamples/nginx-node-redis`
1. Navigate to the project folder using `cd nginx-node-redis`
1. Start the multi-container app using `docker compose up -d --build`
1. Navigate to `http://localhost` to test the multi-container app; if you refresh the browser, you'll see the visit count (tracked in redis) continue to increase
1. Stop the multi-container app using `docker compose down`
