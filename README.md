# WP6T62-02 Federated Data Collection

The objective of this Fractal component is to provide a set of instructions and tools to enable data collection on the Fractal Edge Node.

This component will be based on the usage of open-source existing software and tools to store data (i.e. databases), both for the ARM and the RISCV (64 and 32-bit versions) processor architectures. It will also provide installation steps, and troubleshooting steps in case they are required.

However, as this component is purely based on open-source robust and reliable software, the usage guides about how to perform operations on databases will be only referenced as Documentation, as it would be of overwhelming complexity to give a full comprehensive usage guide for any possible use case scenario.

### How does the component satisfy the WP/FRACTAL objectives?

WP6T62-02 Federated Data Collection component provides a unified set of tools to be implemented in the Fractal Edge Node. These tools cover the storage of any data into the Edge node, and will address the following WP6 objectives (in bold):

- **Analysis of existing open-source edge and cloud platforms implementations in order to determine a potential FRACTAL reference engineering framework.** It will consist of: (i) an edge processing platform and (ii) an edge controller infrastructure.
- Design and implementation of the FRACTAL software processing framework based on microservices for the integration of processing capacities (AI and safe autonomous decision algorithms developed in WP5) and **enabling the connection of IoT devices using standard or proprietary protocols over Ethernet and cellular networks.**
- Edge platform orchestration, monitoring the physical and application integrity at edge locations with the ability to
autonomously enable corrective actions when necessary.
- **Specification of a generic data model and common interface to communicate with the cloud.**
- Design and implementation of the FRACTAL edge node controller infrastructure (in the cloud) for the management
and update of the edge processing platform.
- Adaptation of the FRACTAL software components to allow a fractal growth (scalability) from low to the high
computing edge node.
- Test and validation of the FRACTAL engineering framework by means of simulations and preliminary laboratory
trials (TRL4).
- Integration, test and validation of the FRACTAL wireless communications nodes (4G/5G, NB-IoT/LTE-M, as stand-
alone subsystem (TRL6).

## Getting Started

In this section a set of instructions is given to install and run your data collection components in the different supported processor architectures for the Fractal Edge Node (ARM64, RISCV64 and RISCV32):

### ARM64 & RISCV64 (High and Mid-End Nodes)

ARM is the most popular processor architecture for IoT devices and Edge computing frameworks. This is why it has the most complete set of tools to be installed. For this component, two open source data collection tools are proposed, one relational and one non-relational.

#### Relational Database: CrateDB

A relational database, stores information in tables. Often, these tables have shared information between them, causing a relationship to form between tables. This is where a relational database gets its name from.

CrateDB is a distributed SQL database management system. It is designed for high scalability since it is open source and written in Java, and it includes components from Facebook Presto, Apache Lucene, Elasticsearch, and Netty. CrateDB was developed with the intention of putting IoT data to use and supports IoT data analytics: Time series, AI, geospatial, text search, joins, aggregations, etc.

#### Non-Relational Database: MongoDB

A non-relational database, sometimes called NoSQL (Not Only SQL), is any kind of database that doesn’t use the tables, fields, and columns structured data concept from relational databases. There are various types of [NoSQL databases](https://www.mongodb.com/scale/types-of-nosql-databases).

MongoDB is a document-oriented database software. It is classified as a NoSQL database application. It makes use of JSON-style documents with schemas.

#### Others alternatives

The two databases described above are the recommended databases for this component and the ones that will have an installation guide. However, the list of available databases is very extensive and we would like to provide a list of good alternatives.
* [InfluxDB](https://docs.influxdata.com/influxdb/v2.4/get-started/): NoSQL database for Time-Series data.
* [Apache Cassandra](https://cassandra.apache.org/): High-performance NoSQL database.
* [Apache IoTDB](https://iotdb.apache.org/): High-performance database for Time-Series data. 
* [SQLite](https://www.sqlite.org/index.html): Highly-portable embedded relational database.


### Prerequisites

* CrateDB:
	* Supported OS:
		* Linux, macOS, Windows
	* Docker installation:
		* Docker Engine
		* Download the images from the [Official CrateDB DockerHub](https://hub.docker.com/_/crate/)

* MongoDB:
	* Supported OS: Linux, macOS, Windows
	* Docker installation: 
		* Docker Engine
		* Download the images from the [Official MongoDB DockerHub](https://hub.docker.com/_/mongo/)

### Installation

#### CrateDB
##### Package-based method (Linux)
This method is suitable for Debian, Ubuntu, RedHat and CentOS based systems.

###### Debian/Ubuntu
First of all you will need to add CrateDB package repository to your system.
```shell
# Install prerequisites.
apt-get install sudo
sudo apt-get install curl gnupg software-properties-common apt-transport-https apt-utils

# Import the public GPG key for verifying the package signatures.
curl -sS https://cdn.crate.io/downloads/deb/DEB-GPG-KEY-crate | sudo apt-key add -

# Register with the CrateDB package repository.
[[ $(lsb_release --id --short) = "Debian" ]] && repository="apt"
[[ $(lsb_release --id --short) = "Ubuntu" ]] && repository="deb"
distribution=$(lsb_release --codename --short)
sudo add-apt-repository "deb [arch=amd64] https://cdn.crate.io/downloads/${repository}/stable/ ${distribution} main"
```

Now update the package sources:
```shell
sudo apt update
```

You should see a success message. This indicates that the CrateDB package is successfuly registered. Now you can install CrateDB:

```shell
sudo apt install crate
```

Once installed you can control the `crate` service with `systemctl` utility program:

```shell
sudo systemctl COMMAND crate
```

Replace `COMMAND` with `start`, `stop`, `restart`, `status` and so on.

###### Red Hat/CentOS
First of all you will need to add CrateDB package repository to your system.

```shell
# Install prerequisites.
yum install sudo

# Import the public GPG key for verifying the package signatures.
sudo rpm --import https://cdn.crate.io/downloads/yum/RPM-GPG-KEY-crate

# Register with the CrateDB package repository.
sudo rpm -Uvh https://cdn.crate.io/downloads/yum/7/x86_64/crate-release-7.0-1.x86_64.rpm
```

With everything set up, you can install CrateDB:

```shell
sudo yum install crate
```

Once installed you can control the `crate` service with `systemctl` utility program:

```shell
sudo systemctl COMMAND crate
```

Replace `COMMAND` with `start`, `stop`, `restart`, `status` and so on.

##### Docker

CrateDB and [Docker](https://www.docker.com/) are great matches thanks to CrateDB’s shared-nothing, horizontally scalable architecture that lends itself well to containerization.

In order to spin up a container using the most recent stable version of the official [CrateDB Docker image](https://hub.docker.com/_/crate/), use:

```shell
docker run --publish=4200:4200 --publish=5432:5432 crate
```


#### MongoDB
##### Package-based method (Linux)
Assuming you are using an Ubuntu system, import public GPG Key for MongoDB using:

```shell
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
```

The operation should respond with an OK.

Create the list file `/etc/apt/sources.list.d/mongodb-org-6.0.list` for your version of Ubuntu. The following example is for __Ubuntu 20.04 (Focal)__.

```shell
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

Reload the local package database.

```shell
sudo apt update
```

Install MongoDB packges:

```shell
sudo apt-get install -y mongodb-org
```

Once installed you can control the `mongod` service with `systemctl` utility program:

```shell
sudo systemctl COMMAND mongod
```

Replace `COMMAND` with `start`, `stop`, `restart`, `status` and so on.

##### Docker
MongoDB can run in a container. The official image available on DockerHub contains the community edition of MongoDB and is maintained by the Docker team.

In order to spin up a container using the most recent stable version of the official [MongoDB Docker image](https://hub.docker.com/_/mongo/), use:

```
docker run --name mongodb -d -p 27017:27017 mongo
```

### RISCV32 (Low-End Node)

This guide has been developed with a RISC-V virtual machine with Debian OS running on Qemu with the following specifications:

```
root@debian-riscv64:~# uname -a  
Linux debian-riscv64 5.16.0-5-riscv64 #1 SMP Debian 5.16.14-1 (2022-03-15) riscv  
64 GNU/Linux  

root@debian-riscv64:~# lscpu  
Architecture:          riscv64  
  Byte Order:          Little Endian  
CPU(s):                4  
  On-line CPU(s) list: 0-3  
NUMA:  
  NUMA node(s):        1  
  NUMA node0 CPU(s):   0-3
```

Given the nature of RISC-V, there is a notable lack of package release for the architecture. Thus, the need for compiling source-code is almost always a must for complex applications. Compiling source-code is never a trivial task and providing a guide for it goes beyond the scope of this component.

Instead, we will focus on databases that can be easilly installed and should work as an out-of-the-box solution.

#### MySQL

MySQL is maybe the most popular open-source database and it comes with a release package for Debian on RISC-V architecture:

```
apt-get install mysql-server
```

Once installed you can control the server with `systemctl`.

#### Apache IoTDB

The IoTDB database runs on Java which does have release packages for RISC-V. First we need to install some preliminar packages.

```
apt install default-jdk curl unzip
```

Download and unzip the binaries:

```
wget https://dlcdn.apache.org/iotdb/0.13.2/apache-iotdb-0.13.2-all-bin.zip 
unzip apache-iotdb-0.13.2-all-bin.zip -d
```

We can start the database server with:

```
nohup sbin/start-server.sh >/dev/null 2>&1 &
```

To start a client we can then run the following:

```
# Start the client  
sbin/start-cli.sh -h 127.0.0.1 -p 6667 -u root -pw root
```


## Additional Documentation and Acknowledgments

* CrateDB:
	* [Installation](https://crate.io/docs/crate/tutorials/en/latest/basic/index.html#)
	* [Docs](https://crate.io/docs/crate/reference/en/5.0/)
* MongoDB:
	* [Installation](https://www.mongodb.com/docs/manual/installation/)
	* [Docs](https://www.mongodb.com/docs/manual/)