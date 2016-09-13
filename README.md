# example-docker-compose-01

Example docker containers composed of Nginx and Spring Boot Java application 

[application](/tree/master/application)
[nginx](/tree/master/nginx)

[docker-compose](https://docs.docker.com/compose/)

Launches two containers - for nginx, JVM; that can be configured for local and production use.

Required docker configuration files:

~~~
/docker-compose-base.yml - Shared config that can be extended 
/docker-compose-devl.yml - Localhost deploy 
/docker-compose-prod.yml - Cloud provider deploy 
/nginx/DockerFile - static file
/application/build/docker/DockerFile - generated docker file 
~~~

* Docker Gradle plugin task [com.bmuschko.gradle.docker.tasks.image.Dockerfile](https://github.com/bmuschko/gradle-docker-plugin) generates a DockerFile.
* The build jar is copied to DockerFile context location.
 * Local files can only be added to container from Docker build context location.

~~~
cd application
~~~

Generates java docker file 

~~~
./gradlew createDockerFile
~~~

---

docker-compose.yml is the default filename when --file is not specified.

~~~
docker-compose --file docker-compose-devl.yml build
docker-compose --file docker-compose-devl.yml up      # -d can be appended for detached process
~~~

### Production environment

docker-machine create a docker host on a vm, openstack provider, carina etc

~~~
docker-machine create --driver yourprovider springboot
~~~

2 mins later...

~~~
docker-machine env springboot
docker-compose --file docker-compose-prod.yml up -d
~~~

### Carina

Using Carina - https://app.getcarina.com/app/signup

~~~
export CARINA_USERNAME="username@example.com"

export CARINA_APIKEY="abc..."

carina create cluster001 --wait --nodes=1

carina credentials cluster001

eval $(carina env cluster001)

env | grep DOCKER

docker-compose --file docker-compose-prod.yml up -d
~~~
