Docker Notes


Why need -> 
    Package applications together in a bundle 
    Remove installation and configuration overheads
    Easier CI/CD and maintainance
    Fixed application environment 
    


Image ->
    Packaged code with fixed configuration
    Application Image: eg. Postgres, Redis, etc
    
    Each image can be made from a layer of other images
    Postgres:10.10  (Application Image Layer)
        ^                   ^
    Alpine:3.10     (Linux base Image)


Container ->
    Running environment of an image
    
    Virtual File System
    
    Port Binding
    
    Docker vs VM?  
        Docker is comprised of only the Application Layer
        VM = Application Layer + OS kernet
        
        


Commands:
    docker pull {image}
    docker ps
    docker run
    docker images
    docker start
    docker network
    docker network create mongo-network
    
    docker run -d --name test_redis2 -p 8086:6379 redis
    docker exec -it 0539d318f229 /bin/bash
    
    
    
Workflow with docker:
    >   update code repository
    >   commit with version control sys like git
    >   CI/CD cycle triggered using Jenkins
            Jenkins builds the application and creates artifacts for creating a docker image
    >   Push image to private repository
    
    
Docker Network:
    Docker has capability to set up a seperate internal network for a set of image for easier comms between them
    
    
    
Working Example:
    Run mongodb container
        docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb_main --net mongo-network mongo
    Run mongo express container
        docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb_main mongo-express



Docker Compose File:
    
    Run docker commands in a systematic manner
    Avoid re-entering the cli commands repeatedly to make docker images work
    Takes care of running the dependencies of an image
    
    docker-compose -f docker-compose.yaml up
    docker-compose -f docker-compose.yaml down
    


Docker File
    Used for creating a custom docker image 
  
    alpine -> node -> (our app)
   
    docker build -t my-app:2.0 .
    
    
    
Volumes
    Used for Data Persistance because all data lost when container is stopped/deleted
    
    1. Host Volumes:
        Dictate where on the host file system the reference is made
            i.e. while folder you mount in host file system
        "docker run -v /home/mount/data:/var/lib/mysql/data
    
    2. Anonymous Volumes:
        We only specify the folder in the container which needs to be persisted,
        we don't care about where its persisted in the host machine
        "docker run -v /var/lib/mysql/data"
    
    3. Named Volumes:
        Reference the volume by name
        "docker run -v name:/var/lib/mysql/data"