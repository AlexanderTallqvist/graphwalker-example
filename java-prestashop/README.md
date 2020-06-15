# This project was made for a presentation at  [SAST](http://sast.se/meeting.jsp?id=381)

The example uses [PrestaSop](https://www.prestashop.com/en) as the System Under Test [SUT].
To get the the SUT installed locally the presentation uses the docker version of PrestaShop. Go to [PrestaShops GitHub
page](https://github.com/PrestaShop/PrestaShop) and follow the instructions there.  

Before running the example, make sure following requirements are met/installed on your machine:
* Java JDK 8
* Maven >= 3.5
* Docker >= 18 
* Firefox latest version

## Get the PrestaShop demo running

The instructions are from: https://hub.docker.com/r/bitnami/prestashop/

```shell script
mkdir PrestaShop && cd PrestaShop
curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-prestashop/master/docker-compose.yml > docker-compose.yml
docker-compose up -d
```

To shut it down:

```shell script
docker-compose down
```

The `docker-compose up` command will launch 2 services, one database and the PrestaShop web app. It will take a while until all is up running.

Goto http://localhost, and the last install phase of Prestashop will start.

![alt tag](images/prestashop/bitnami_installing.png)

When done, you will be forwarded to:

![alt tag](images/prestashop/After_installation.png)


## GraphWalker running tests using different test tools

Extending the same interface `PrestaShop`, different implementations can be used to run th same tests. The interface is created from models in the folder<br>
 `src/main/resources/com/prestashop`<br>
and will end up in this folder<br>
 `target/generated-sources/graphwalker/com/prestashop`.

Interfaces are created by graphwalker by running:<br>
```shell script
mvn graphwalker:generate-sources
```
They are also created during the `compile` lifecycle of maven. 


### Run the GraphWalker test with [Selenide](https://selenide.org/)

The model implementation using [selenide](https://github.com/GraphWalker/graphwalker-example/blob/master/java-prestashop/src/main/java/com/prestashop/modelimplementation/SelenideImpl.java).

```shell script
git clone https://github.com/GraphWalker/graphwalker-example.git
cd graphwalker-example/java-prestashop
mvn -Pselenide compile exec:java -Dexec.cleanupDaemonThreads=false -Dexec.mainClass="com.prestashop.runners.SelenideRunner"
```

### Run the GraphWalker test with [Eye](https://eyeautomate.com/eye/)

The model implementation using [eye](https://github.com/GraphWalker/graphwalker-example/blob/master/java-prestashop/src/main/java/com/prestashop/modelimplementation/EyeImpl.java).

You need to install eye2.jar in order for the below to work

```shell script
mvn install:install-file -Dfile=<PATH TO JAR>/eye2.jar -DgroupId=eye -DartifactId=Eye -Dversion=2 -Dpackaging=jar

mvn -Peye compile exec:java -Dexec.cleanupDaemonThreads=false -Dexec.mainClass="com.prestashop.runners.EyeRunner"
```

### Run the GraphWalker test with [SikuliX](http://sikulix.com/)

The model implementation using [sikuli](https://github.com/GraphWalker/graphwalker-example/blob/master/java-prestashop/src/main/java/com/prestashop/modelimplementation/SikuliImpl.java).

```shell script
mvn -Psikuli compile exec:java -Dexec.cleanupDaemonThreads=false -Dexec.mainClass="com.prestashop.runners.SikuliRunner"
```
