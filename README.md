Example Voting App
A simple distributed application running across multiple Docker containers.

Getting started
Download Docker Desktop for Mac or Windows. Docker Compose will be automatically installed. On Linux, make sure you have the latest version of Compose.

Linux Containers
The Linux stack uses Python, Node.js, .NET Core (or optionally Java), with Redis for messaging and Postgres for storage.

If you're using Docker Desktop on Windows, you can run the Linux version by switching to Linux containers, or run the Windows containers version.

Run in this directory:

docker-compose up
The app will be running at http://localhost:5000, and the results will be at http://localhost:5001.

Alternately, if you want to run it on a Docker Swarm, first make sure you have a swarm. If you don't, run:

docker swarm init
Once you have your swarm, in this directory run:

docker stack deploy --compose-file docker-stack.yml vote
Windows Containers
An alternative version of the app uses Windows containers based on Nano Server. This stack runs on .NET Core, using NATS for messaging and TiDB for storage.

You can build from source using:

docker-compose -f docker-compose-windows.yml build
Then run the app using:

docker-compose -f docker-compose-windows.yml up -d
Or in a Windows swarm, run docker stack deploy -c docker-stack-windows.yml vote

The app will be running at http://localhost:5000, and the results will be at http://localhost:5001.

Run the app in Kubernetes
The folder k8s-specifications contains the yaml specifications of the Voting App's services.

First create the vote namespace

$ kubectl create namespace vote
Run the following command to create the deployments and services objects:

$ kubectl create -f k8s-specifications/
deployment "db" created
service "db" created
deployment "redis" created
service "redis" created
deployment "result" created
service "result" created
deployment "vote" created
service "vote" created
deployment "worker" created
The vote interface is then available on port 31000 on each host of the cluster, the result one is available on port 31001.

Architecture
Architecture diagram

A front-end web app in Python or ASP.NET Core which lets you vote between two options
A Redis or NATS queue which collects new votes
A .NET Core, Java or .NET Core 2.1 worker which consumes votes and stores them in…
A Postgres or TiDB database backed by a Docker volume
A Node.js or ASP.NET Core SignalR webapp which shows the results of the voting in real time
Note
The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.
