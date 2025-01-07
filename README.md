## Microservices Project
This project consists of two locally developed solutions containerized and deployed in the same Kubernetes cluster, communicating with each other using RabbitMQ message bus service through an Nginx configured API gateway. The service models have been mapped on to each other using AutoMapper. While the Platform Service has been configured to persist in a SQL Server database, the Command Service uses an In-Memory database to issue commands and run requests.

### Setting Up The Project:
This is a Docker and Kubernetes intensive project. For this reason, it is highly recommended to use Docker Desktop as it integrates smoothly with Kubernetes. For API testing, Postman API has been preferred. The use of SQLServer is optional as it is used to verify existing data objects, but strictly speaking is not necessary. You do not need a server instance as the server used inside our cluster is deployed using Kubernetes. The links to download all are listed below:
- [Download Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Download Postman API](https://www.postman.com/downloads/)
- [Download SQL Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) (optional)

Once the downloads are complete, go ahead and pull the project to local system. There are 3 folders:
- CommandService
- K8S
- PlatformService

To get started, start Docker Desktop and enable Kubernetes. All services needed to run the project have been packed as **.yaml** files inside the K8s folder, which can be deployed once Kubernetes is up and running. To start deploying the yaml files, navigate to the K8S folder using terminal of choice, and enter the commands in the following order to deploy the services one-by-one:
```
kubectl apply -f plaftorms-depl.yaml
kubectl apply -f commands-depl.yaml
kubectl apply -f rabbitmq-depl.yaml
kubectl apply -f mssql-plat-depl.yaml
kubectl apply -f local-pvc.yaml
kubectl apply -f ingress-srv.yaml
kubectl apply -f plaftorms-np-srv.yaml
```


### Launching MongoDB:
The next step is to launch an instance of a MongoDB Docker container. Using terminal of choice, enter the following code:
```
docker run -d --name <nameofproject> -p 27017:27017 -v <nameofdatabase>:/data/db mongo
```

### Interacting with the API:
The last step is to interact with your REST API. To do this, run the project from local repository.
Once the project is running, open Postman API. In the address bar, type the following:
```
https://localhost:5001/items
```
Now you're ready to interact with the API!

### Request Types:
At first, your database will be empty because you haven't added any items. Test out the API in the following request sequence for the best experience.
#### POST
Start with a POST request to add some items to the Catalog. Make sure to send a raw JSON object in the body using the following format:
```
{
  "name" : "<Name of Object>" (string)
  "price" : (decimal or int)
}
```
This will generate your item with a Guid as ID, the name and price you specified, and the date of creation.
#### GET
A GET request will return all the items you added to your database using the POST method.
#### GET {with id}
If you paste the id of item at the end of a GET request, you can retrieve that specific item.
#### PUT {with id}
A PUT request with the id of the item, and a raw JSON like the one shown in the POST method will allow you to modify that existing item to change its name or price.
#### DELETE {with id}
A DELETE request with the id of the item will allow you to delete that particular item.
