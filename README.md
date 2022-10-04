<!-- omit in toc -->
# Demo development environment for deploying microservices
The repo serves as the basis for a docker setup of a deployment architecture for microservices development. Web service (frontend) and API (backend) are implemented in .NET 6 as an example, but can be exchanged quickly and easily.
The readme describes a quick setup of the whole environment, as well as further details to reproduce this or a similar environment in the second part.  

**Altough this contains an environment for "production", don't use it for real production purposes (e.g.: certificates are self signed and publicly available in this repo...).**

- [What to expect?](#what-to-expect)
  - [Webservice](#webservice)
  - [Backend](#backend)
  - [Monitoring](#monitoring)
- [Fast setup of a complete production environment](#fast-setup-of-a-complete-production-environment)
  - [Prerequisites](#prerequisites)
  - [Installation of prerequisites](#installation-of-prerequisites)
    - [Linux (Debian)](#linux-debian)
    - [Windows](#windows)
  - [Build environment for production step with Docker](#build-environment-for-production-step-with-docker)
  - [Build environments from other production steps](#build-environments-from-other-production-steps)
- [Advanced configuration](#advanced-configuration)
  - [Private Docker registry](#private-docker-registry)
    - [local](#local)
    - [remote](#remote)
  - [Frontend Webservice](#frontend-webservice)
  - [API](#api)
  - [Mock API](#mock-api)
  - [Database](#database)
  - [Nginx](#nginx)
  - [Grafana & Prometheus](#grafana--prometheus)
- [Customization](#customization)
  - [Rebuild and replace services](#rebuild-and-replace-services)
  - [Change database](#change-database)
  - [Grafana and Prometheus](#grafana-and-prometheus)
  - [Network](#network)
- [Default credentials](#default-credentials)
- [Todo / out of scope for now](#todo--out-of-scope-for-now)

## What to expect?
A complete development environment with functional demo services (web frontend, API, database), a mocked backend (Swagger/OAS 2.0), reverse proxy/load balancer and monitoring (Grafana, Prometheus, Cadvisor). All services are containerized. The container composition and network configuration for the deployment steps local development, integration testing, end to end testing, staging and production are stored in separate docker-compose files.  
The specified ports refer to the production setup, which is described in the next step. In the setups of the other steps, the ports may differ (see detailed setup).

| ![image](assets/images/deployment.png) |
|:--:|
| *Reusing containers during deployment steps* |

| ![image](assets/images/physical.png) |
|:--:|
| *Basic model of physical components and networking* |


### Webservice
A simple web page to retrieve, modify, add and delete users from a database.  

**Access via https://hostip**

| ![image](assets/images/webservice-start.png) |
|:--:|
| *Start page requesting all users* |

| ![image](assets/images/webservice-add.png) |
|:--:|
| *Adding a user (includes valid mail valdiation check)* |

### Backend
A minimal API implemented in NET (using swagger UI to show endpoint options), a MySQL (Maria) database with sample data already setup and a container running a mocked API (config file is pulled from repo and can be changed on runtime).

**None of the backendservices are directly accessible in the default setup. Check detailed setup section for further information on how to access these containers.**

| ![image](assets/images/swagger.png) |
|:--:|
| *Swagger UI of the API container* |

| ![image](assets/images/swagger2.png) |
|:--:|
| *Demo request to /users endpoint showing the sample data* |

| ![image](assets/images/mocked.png) |
|:--:|
| *Same endpoint /users requested from mocked API container* |

| ![image](assets/images/database.png) |
|:--:|
| *Sample data in Maria DB container (mounted to data directory in db directory)* |

### Monitoring
To represent as realistic a scenario as possible, a stack for visualization (Grafana), time-based data collection (Prometheus) and container monitoring (CAdvisor) is provided for monitoring. The necessary configuration has already been done.

**Access:  
Grafana: http://hostip:3000, user: admin, pw: grafana  
CAdvisor: http://hostip:8080/docker/containername**

| ![image](assets/images/grafana.png) |
|:--:|
| *Network monitoring in Grafana. The dashboard shown is setup in the mounted dashboard.json file* |

| ![image](assets/images/cadvisor.png) |
|:--:|
| *Container monitoring in CAdvisor* |

## Fast setup of a complete production environment
### Prerequisites
- Docker
- Docker Compose
- Docker service running
- Git

### Installation of prerequisites
#### Linux (Debian)
- ```curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh```
- ```sudo spt update```
- ```sudo install docker-compose```
- if you want Docker to start after rebooting run ```sudo systemctl enable docker.service``` and ```sudo systemctl enable containerd.service```
- check if docker is running with ```service docker status```. If it is not, run ```service docker start``` and check again
- if git is not installed already run ```sudo apt install git```

#### Windows
- run ```DISM /online /enable-feature /featurename:Microsoft-Hyper-V-All``` as admin or search for advanced features and activate Hyper-V
- download and install Docker Desktop
- download and install git

### Build environment for production step with Docker
- ```cd``` or ```mkdir``` (to) a directory of your choice
- run ```git clone --recurse-submodules https://github.com/HansenBerlin/docker-demo-devenv.git``` from within that directory
- run ```cd docker-demo-devenv``` to change to the directory that was created by git
- run ```docker-compose up -d``` to start all services. This might take some time the first time you do this - depending on the images already available in your local registry and your internet connection.
- after everything is setup (you should see the prompt in your CLI again, without any errors being shown), open https://yourhostip in your browser
- if everything worked, you can access the services via the ports described in the previous step

### Build environments from other production steps
Within the composefiles directory you can find several docker-compose files. ```cd``` into the directory and run ```docker-compose up -d``` to run the appropriate containers for the chosen environment. For a teardown of all containers run ```docker-compose down``` from the same directory you ran the first command.   
If a database is used (like in fe-dev-local, staging and endtoend testing) the data folder witihin the directory is populated with the database content. After teardown the **files are not deleted and reused** when spinning the containers up again. If you want a clean database with the sample data, just delete the content of the data folder before running ```docker-compose up -d```.  
**Some files point to a private registry that might be shutdown in the future and might stop working.**

## Advanced configuration
tbd
### Private Docker registry
tbd
#### local
tbd
#### remote
tbd
### Frontend Webservice
tbd
### API
tbd
### Mock API
tbd
### Database
tbd
### Nginx
Nginx directory file structure:
certs/  
├─ localhost.crt  
├─ localhost.key  
conf/  
├─ default.conf  
Dockerfile  

tbd
### Grafana & Prometheus
tbd
## Customization
tbd
### Rebuild and replace services
tbd
### Change database
tbd
### Grafana and Prometheus
tbd
### Network

## Default credentials
dc: root, secret  
grafana: admin, grafana  
registry: testuser, testpassword  

## Todo / out of scope for now
- integrate build server (Jenkins or TeamCity)
- add custom dashboards to Grafana
- extract ORM service for more generic database access
- add manual for setup with cloud providers
- change path to Dockerfile instead of private registry in docker compose files























