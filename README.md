# Gluu Server Deployment Guide


This guide focuses on planning the efficient Gluu Server deployment strategy for your
 organization/ application. Gluu server supports single server/ VM/ container-based
deployment, cluster manager based deployment, and up to the highly scalable solution
relying on Kubernetes and Clouchbase.
The underlying innovative technologies have enabled the Gluu server in processing a
billion authentications per day. Gluu server relies on the cloud-native technologies,
 hosted Kubernetes like Amazon Elastic Kubernetes Services (EKS)  and Couchbase database
 to process a billion authentication a day. It also uses cost-effective autoscaling to
 meet the sudden demand without any extra pre-provisioning.

## Introduction of Gluu Server   

Gluu Server is a free open source software (FOSS) that enables its users to control
access to their resources. In a nutshell, the Gluu Server is an efficient Identity and
 Access Management (IAM) solution. Any multi-user web/ mobile application that requires
  user authentication, identity management and resource access policies can leverage
  a Gluu server.  
**Open Source Gluu Server**
- Gluu Server is an open-source software consisting of software written by Gluu and
incorporated by other open-source projects. Gluu also supports the open-source as:
The source code is a free open-source with several essential interception scripts written
by Gluu to implement the custom business logic for authentication and authorization.
Gluu server binaries are free for different distributions (packages, containers, K8, snap etc.).

- Gluu Server documentation is freely available.

- Gluu offers free and VIP support. The Gluu support portal is open to browse, register
and post questions. The support portal (https://support.gluu.org) serves as a knowledge
base for the users. The VIP support comes under a paid contract with guaranteed response
time and consultative support (more details: https://www.gluu.org/pricing/).

**Open Standards behind Gluu Server**

Gluu Server uses several open standards. Subsequent paragraphs discuss a brief detail
 of these standards.

**OAuth:** OAuth is an open-standard authorization protocol for authorization. It describes
the way of authenticated access among unrelated servers. OAuth 2.0 is an industry-standard
authorization protocol that focuses on client developer simplicity and providing authentication
flows for different applications.

**OpenID Connect:** OpenID connect (OIDC) is an identity layer on top of OAuth 2.0 protocol.
OIDC enables the users to verify users’ identity, which was authenticated by an authorization
server or identity provider (IDP). OpenID Connect specifies a RESTful HTTP API using
JSON as a data format.

**SAML:** Security Assertion Markup Language (SAML) is a standard for Web-based single
sign-on (SSO). SAML is an XML-based open-standard for exchanging authentication and
authorization between the IDP and service provider (SP).

**UMA:** User-Managed Access (UMA) is an OAuth-based access management protocol standard.
The purpose of UMA is to enable the resource owners to control the data sharing and
protected resource access among the online services on the owner’s behalf.

**RADIUS:** RADIUS is an AAA (Authentication, Authorization, and Accounting) protocol
to manage network access. RADIUS uses two packet types, namely Access-Request for
authentication and authorization and Accounting-Request for third A, accounting.

**SCIM:** System for Cross-Domain Identity Management (SCIM) is a standard to automate
the user identity exchange on the web and in cross-domain applications such as enterprise
to cloud service providers or inter-cloud services.

**CAS:** Central Authentication Service is an SSO protocol for web-based applications.
CAS protocol mainly involves a client (usually a web browser), a web application requesting
the authentication, and a CAS Server.

**Gluu Server Design Goals**
The following are the underlying design goals behind Gluu server development.

**High Concurrency:**  The main constraint of authorization platforms is not the number
of users, but the number of transactions per second. In order to support high
concurrency, you need to support horizontal scalability. The Gluu web tier is
stateless. Gluu also supports persistence and caching tiers that can expand based
on load.

**High Availability:**  Gluu supports active-active multi-datacenter deployments.

**Flexibility:**  Many organizations have business specific requirements during
the authentication and authorization phase. The Gluu team has developed several
interception scripts to enable the customization without forking the Gluu server
code to achieve flexibility.

**Low cost of operation:** Over time, and IAM systems are typically in production
for around a decade, the operating cost is the largest component of total cost
of ownership. By providing standard deployment strategies, Gluu reduces operational
cost by providing commonality across deployments. Reducing one-off's is the
best way to reduce cost.

## Common Use-cases
This section presents brief details of the typical use cases of the Gluu Server.

**SSO:** Single Sign-On (SSO) is an authentication scheme that enables the related
yet independent applications to allow users to use a single user id and password for
login and access them. This scheme also allows the users to sign in only once to
access all the related applications.

**API Access Management:** API Access Management helps ensure that only authenticated
calls will access the APIs.

**Mobile Authentication:** It is a user identity verification method using a mobile
device and at least one more authentication method for secure access.
IoT: Gluu server efficiently manages the authentication requirement of IoT applications
involving a wide range of heterogeneous devices. Gluu Server is suitable for the IoT
applications using TCP-based communication.

**Strong Authentication:** Gluu server supports various strong authentication
mechanisms, including U2F keys, OTP apps, and a free and secure mobile push app Super
Gluu.  

## Identity Components Gluu Server

**oxAuth:** oxAuth is an open-source OpenID Connect Provider (OP) and UMA Authorization
Server (AS). The project also includes OpenID Connect Client code, which can be used
by websites to validate tokens.

oxAuth is the web tier of the Gluu Server; it is stateless and can be scaled horizontally.

oxAuth currently implements all required aspects of the OpenID Connect stack, including
an OAuth 2.0 authorization server, Simple Web Discovery, Dynamic Client Registration,
JSON Web Tokens, JSON Web Keys, and User Info Endpoint.

**oxTrust:** oxTrust is a Weld based web application for Gluu Server administration.
oxTrust enables administrators to manage what information about people is open to
partner websites.

oxTrust is also the local management interface that handles other server instance-specific
configurations and provides a mechanism for IT administrators to support people at the
organization who are having trouble accessing a website or network resource.

**Passport-js:** Gluu bundles the Passport-js authentication middleware project to
support user authentication at external SAML, OAuth, and OpenID Connect providers known
as inbound identity.

The integration consists of custom authentication scripts and web pages for oxAuth,
and the Passport Node.js application. Together, these assets coordinate the flows and
interfaces required to configure the Gluu Server to support inbound identity with a
range of external providers.

**Casa:** Gluu Casa ("Casa") is a dashboard for users to manage authentication and
authorization data in the Gluu Server. Casa is a self-service web portal for end-users
to manage authentication and authorization preferences for their account in a Gluu Server.

**oxauth-rp:** The Gluu Server ships with an optional OpenID Connect RP web application
called Oxauth-rp, and it is useful for testing. During the Gluu Server setup, the user
may opt to install it, and we recommend it for a development environment. Using this
tool, you can exercise all of the OpenID Connect APIs, including discovery, client
registration, authorization, token, user info, and end_session.

**oxd:** oxd exposes simple, static APIs for web application developers to implement
user authentication and authorization against an OAuth 2.0 authorization server like
Gluu.

**Super Gluu:** Super Gluu is a push-notification two-factor authentication (2FA)
mobile application. Super Gluu uses public-key encryption as specified in the [FIDO
U2F authentication standard](https://fidoalliance.org/specifications/overview/).
Upon device enrollment, Super Gluu registers its public key against the Gluu Server's
FIDO U2F endpoint. When authentication happens, there is a challenge-response to ensure
that the device has the corresponding private key.

**Gluu Gateway:** Gluu Gateway (GG) is an authentication and authorization solution
for APIs and websites. GG bundles the open-source [Kong Gateway](https://konghq.com/community/)
for its core functionality and adds a GUI and custom plugins to enable access
management policy enforcement using OAuth, UMA, OpenID Connect and Open Policy Agent
(OPA). GG also supports the broader ecosystem of [Kong plugins](https://docs.konghq.com/hub/)
to enable API rate limiting, logging, and many other capabilities.

**Cluster Manager:** Cluster Manager (CM) is a GUI tool for installing and managing
a highly available, clustered Gluu Server infrastructure on physical servers or Virtual
 Machines (VMs).  Gluu Cluster Manager does not support CloudNative container based
 installations such as Docker and Kubernetes.

Cluster Manager can be configured and used to automatically synchronize security
updates, configuration changes, user and data object changes, LDAP schema changes,
automate key rotation, centrally manage logs, as well as automating the number of
other mission critical human error prone  operational support functions.

## Persistence

Gluu abstracts persistence, enabling implementations to connect to different
databases. The database stores configuration, user profiles, tokens, and
other data needed for authentication and authorization.

### Gluu LDAP

Gluu maintains a fork of OpenDJ in [Github](https://github.com/GluuFederation/gluu-opendj4).
This is the default database when you install Gluu Community Edition.  OpenDJ supports multi-master replication (MMR)--i.e. changes to any instance propagate to other instances.

#### Operational Considerations for LDAP

1. Indexing: All searches with scope sub | one must be index. By observing the logs
a user will get hints on which searches should be indexed. The higher etime with unindexed searches signals the performance issues.
1. Replication:  For replication LDAP server behaves like master and slave. The slave (replica) server will help in avoiding single point of failure by acting as a backup (replica) and in improving the performance by sharing the load with the master server.
1. Backup-restore: Backup helps in recovery from any unanticipated failures. Mainly there are two options of backup i.e. full backup and icremental bacup. Both the full and incremental backup is recommended as a best practice. There are two options for backup as binary backup and ldif backup.
1. Suffixes
  1. **o=gluu** This is the main database for storing Gluu runtime operational data.
  1. **o=site** This is the database used by the cache-refresh process for
  synchronizing user data from external ldap servers.
  1. **o=metric** This is a Gluu suffix to store data about performance.
  1. **cn=monitor** This is a built-in suffix which collects operational statistics.
  1. **cn=config** This is the internal ldap configuration and should not be
  modified. Use the `dsconfig` tool to manage OpenDJ configuration.
1. Directory Namespace (DIT): The Namespace or Directory Information Tree (DIT) is the convention for naming the entries in the LDAP directory.  
1. Monitoring: Monitoring involves statistics gathering, event monitoring, logging and alerting.  
1. Logging: Logging helps in troubleshooting of errors and any performance issues.  
1. Management tools:
1. Schema management:

### Couchbase

Couchbase Enteprise Edition (not Community Edition) is an alternate
database which is recommended to achieve high concurrency. LDAP can handle large
data sets, even millions of users, if concurrency is relatively low, for example,
less then 100 authentications per second. Couchbase can be scaled horizontally--
you can split the index, query, analytics and data nodes onto different servers,
increasing capacity. Also, when you add more data nodes, the data is automatically
redistributed, enabling you take advantage of more disks without manual steps to
move data around.  Couchbase also includes a cache service, which can eliminate
the need for a separate cache component (like Redis), thus reducing the operating
cost of another piece of infrastructure.


#### Operational Considerations for Couchbase

1. Indexing: Couchbase indexes improves the performance of query and search operations. Couchbase supports several forms of indexes as primary, secondary, functional,  full text, etc. During index creation selection of key, order and expression are important attributes to be considered.   
1. Replication: Couchbase supports two forms of replication namely local/ intra-cluster replication and remote/ cross data center replication (XDCR).
1. Buckets: The Couchbase buckets are used to store the data persistently or in Memory. The buckets supports auto replication and dynamic scaling using well defined protocols and mechanisms like Database Change Protocol (DCP) and Cross Data Center Replication (XDCR).   Couchbase server uses the buckets to logically group collection of keys and values. A Couchbase cluster supports a maximum of 30 buckets.
1. Backup-restore: Couchbase has multiple options for backup and restore specially using the backup manager (cbbackupmgr) or using backup and restore tools (cbbackup and cbrestore).
1. Monitoring: Couchbase server has a decent web-based administration console to monitor the collected data, visualizing it and creation of alerts. It also has REST based queries for historic and current statistics stored in the cluster. Moreover, Couchbase server also stores per node statistics.   
1. Logging: Couchbase logging facility allows to store important Couchbase server events. It records these events and stores them in log files. The Couchbase web console also displays the cluster-wide significant events on the log screen.  
1. Schema management:
1. Management tools:


### RDBMS

Gluu Server will add RDBMS's support in Gluu Server 5.x. The reason
it wasn't added in the past is because the performance and replication features
of LDAP was better for authorization. But with improvements to RDBMS, it's becoming
a more viable option, especially for mid-size deployments.

## Caching

Persistence is the main performance bottleneck--writing data to the disk is the
slowest operation. Reading data from the disk is a little faster, but still
relatively slow. Some data is just not worth writing to the disk. For example,
the code in the authorization code flow is only good for one-time use. If a
code is lost, the impact on the user is low (just reload the page). So it makes
sense to cache data like this instead of writing it to the disk. Another example
of data that can be cached are access tokens, which are generally good for less
then five minutes.

The Gluu Server supports the following four methods for caching of session data
and generated tokens.

**In-memory:** In-memory caching is recommended only for single server deployments.

**Redis:** This is recommended in combination with LDAP. For example, you want
to use LDAP, but you need to improve the performance. One challenge with Redis
caching is that you need to build a robust topology of Redis servers, so the
Redis service itself does not become a single point of failure.

**memcached** Similar use case to LDAP, for organizations that do not want to use
Redis. Gluu has found that memcached has a high cache miss ratio--as much as 20%--
under load. So if possible, we recommend Redis.

**Native Persistence (Couchbase, LDAP):** This is an oxymoron. Using a database
for caching is the opposite of a cache! The reason for this option is that session
sharing is needed in a cluster, but the concurrency requirement is low, so it
is preferable to avoid the operational complexity of deploying a highly available
cache infrastructure. For example, using LDAP with multi-master replication,
which is already in place, may reduce operational cost.

**Couchbase** For high concurrency requirements, where Couchbase is already the
database, organizations can leverage their In Memory buckets, which is basically
a key-value lookup cache service.

## Suggested Deployment Architecture

The actual Gluu Server deployment depends on the application. Several factors (size
of the application in terms of number of users, number of authentication per day, etc.)
affect the deployment architecture. Mostly the applications can be efficiently served
by single server deployment. There may be some mission-critical and large application
that requires the highly available multiple-server/ cluster-based Gluu Server deployment.

**Single Server Deployment:** There are several ways a Gluu Server is deployed on a single server instance as discussed in subsequent paragraphs.

- **Single Server VM / Linux Packages chroot distribution:** A user can install the
Gluu Server inside a VM of physical servers with at least 2 CPU units and 4 GB RAM.
The Gluu Server is a Chroot container. Chroot is a UNIX operation that changes the
root directory of the current running process and its child processes. The running
process under this modified environment cannot access the files outside the assigned
directory tree. Chrooted environment provides file system isolation to the processes
but is different than the container environment like LXC or Docker as it shares other
resources of host for example IP address.

**Single Server VM / Linux -nochroot distribution:** Beginning with version 4.2.1,
Gluu server Community Edition is available for install directly to the operating system
without the use of the chroot container. Same hardware specifications are used for the
nochroot install.

- **Snap Package:** A Canonical product named as Snap is a software packaging and
deployment system for the Linux kernel-based operating systems. The snap package for
Gluu-ce is named as gluu-snap. This package will have the same structure like the RPM
or DEB of Gluu-ce.
Cloud-based Gluu Server Deployment: Gluu server supports most of the cloud providers,
a few selected providers are as:

- **Digital Ocean:** Gluu Server can be deployed on the digital ocean cloud using
containership.io. Containership is a containers-as-a-service platform that makes it
easy to deploy, manage and scale containerized applications on any public or private
cloud.

- **Amazon AWS:** Gluu Server also supports installation on Amazon AWS instances with
the private address.

## Cluster Deployment

Gluu Server also supports the creation of high available deployment using multiple
instances on VMs/ physical server. The Gluu Server cluster is an efficient way to
manage the requirements of high demanding applications. 

### Requirement of a Good Cluster

Configuration of a good cluster is a complex and challenging activity. Several
requirements of a good cluster require careful consideration during the planning. 
This subsection discusses these requirements of an ideal cluster. It is possible to
use one server for multiple functionalities like the Gluu server node, load balancer,
cache server, etc. However, we recommend using a separate server for these components.

**Load Balancer:** The load balancer acts as front-end to efficiently interact with
the Gluu server clustered nodes. Organizations can  load balance using any hardware,
software or managed service. Gluu is stateless, so any load balancing algorithm will
work, including round robin.

**Replication:** Several Gluu Server components should be replicated, specifically the
component related to the storage and associated functionalities. The database, cache,
and filesystem should have their replicas for high performance and availability. An
important activity with a replicated component is setting proper synchronization
mechanisms among the replicas of some components.  

- **Database:** The database stores authentication-related information and it should
have at least one replica to avoid a single point of failure. Each replica will be
having the entire authentication-related information.

- **Filesystem:**  Gluu server uses Csync2 to perform the file system replication of
clustered nodes, to replicate the configuration files, metadata files, and certificates,
etc.   

- **Cache:** There should be at least one cache server and there can be multiple cache
(Redis) server for high performance.

**Certificate and Key Management:** Most of the Gluu Server components have cryptographic
keys and X.509 certificates which are stored in chroot. They may have different key
formats and Keystore formats that should be properly managed.

**Logging:** To support the troubleshooting of any issues, logging on all the components
(oxAuth, oxTrust, HTTPD (Apache2), WrenDS, and Redis) of Gluu Servers is an essential
requirement.  

**Benchmarking of Each Component:** Benchmarking each of the Gluu Server components
is an essential requirement of a high-performance cluster. Each application has
different performance requirements thus it is good to do separate benchmarking of
each component to verify that it achieves the desired performance.  

**Ease of Deployment and Maintenance:** From the user's perspective, all of the above
requirements and functionalities should be easier to deploy and maintain. Gluu Server
has an effective and efficient cluster deployment offering and it is discussed in
subsequent paragraphs in this section.

### Gluu Server Cluster Deployment Models

Gluu Server supports various active-active high availability deployment models.

**Manual:** A difficult to configure choice for a high available Gluu Server cluster
across multiple instances is to configure it manually. The cluster requires a minimum
of four servers or VMs as two for Gluu Server, one for load balancing and one for redis.
This requires a lot of effort towards making sure that the deployment meets the performance
requirement. The individual components must be properly tested and benchmarked for the
performance requirement. Note*. Manual cluster configuration is not eligible for Gluu
support.

**Cluster Manager:** Cluster Manager (CM) is a GUI tool for installing and managing a
highly available, clustered Gluu Server infrastructure on physical servers or VMs. CM
can be used to cluster an existing single node Gluu Server, or can be used to deploy
a new cluster of Gluu Servers from scratch. CM automates several tasks associated with
setting up and managing the high available cluster including its installation, database
replication, cache management, logging, secure tunneling between oxAuth and cache
server (Redis).

**Docker-Kubernetes Cloud Native Implementation:** Kubernetes is an open source
container-orchestration system to automatically deploy, scall and manage the application.
Gluu Server supports configuration of both cloud (amazon’s EKS, Google’s GKE, DigitalOcean’s
  DOKS, Microsoft AKS etc.) and local kubernetes (Minikubes, MicroK8s) clusters.

## Sizing
Gluu Server supports applications of various sizes from a few hundreds/ thousands of
authentications per day to a billion authentications per day. There are several
parameters that affects the sizing decisions, a carefull consideration of these
parameters helps in taking selecting aprorpiate sizing for the organization/ applications.
This section will present an overview and in-depth discussion of these parameters
covering how they affect the Gluu server's deployment and performance. These
parameters are the total number of users (active & idle), desired authentication
throughput, type of authentication application workflow, preferred persistence method
(database and caching) technique, desired redundancy model, and the deployment stack
like VM/ physical server-based to Cloud-native and Kubernetes based deployment.

The following table presents details of these parameters and their impact on the performance.

|Parameter        |It's Impact              |
|-----            |-----------              |
|Number of Users      |As the database size grows with the users as active and inactive users, occupy space. The number of active users also affects the caching size, and ultimately it also plays an important role in the physical memory size.|
|Desired Authentication Throughput|The desired authentication throughput is an essential parameter to take the sizing decisions as Gluu Server supports low, medium, high to extremely high authentication throughput. Gluu Server deployment on a single VM/ server will meet the low and medium authentication throughput demands. For extremely large (a billion authentication per day), we recommend Gluu Server deployment with Couchbase, Cloud-Native, and Kubernetes.|
|Application Workflow    |The application flow also affects the sizing decision as a single Gluu Server instance (VM/ Physical Server) can efficiently handle the demand of the applications that require simple authentication token generation. For complex application workflows that may require multistep authentication, the involvement of AI for fraud detection, etc. one could think of more sophisticated deployment involving cluster, caching, and efficient persistence method.|  
|Preferred Persistence Method     |Gluu Server supports different persistence methods to cope up with various applications' demands. It includes several combinations of different databases and caching to support applications of various scales.|
|Desired Redundancy Level        |The opted redundancy model obviously affects the size of deployment in terms of the number of instances.|
Preferred Deployment Stack        |The different Gluu Server deployment stacks have their own requirement for the number of instances & configuration of each instance. Accordingly, they support a different level of performance.| 

There are several possible deployment stack for Gluu Server as:

- **VM:**
- **Kubernetes:**
   - **Couchbase:**
   - **LDAP/ Redis:**

## Recommended Deployment Strategy

## Common Issues and FAQs

## Deployment Best Practices

## Summary and Further References
