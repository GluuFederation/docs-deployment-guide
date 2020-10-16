# Gluu Server Deployment Guide
   

This guide focuses on planning the efficient Gluu Server deployment strategy for your organization/ application. Gluu server supports single server/ VM/ container-based deployment, cluster manager based deployment, and up to the highly scalable solution relying on Kubernetes and Clouchbase.
The underlying innovative technologies have enabled the Gluu server in processing a billion authentications per day. Gluu server relies on the cloud-native technologies, hosted Kubernetes like Amazon Elastic Kubernetes Services (EKS)  and Couchbase database to process a billion authentication a day. It also uses cost-effective autoscaling to meet the sudden demand without any extra pre-provisioning. 

## Introduction of Gluu Server   

Gluu Server is a free open source software (FOSS) that enables its users to control access to their resources. In a nutshell, the Gluu Server is an efficient Identity and Access Management (IAM) solution. Any multi-user web/ mobile application that requires user authentication, identity management and resource access policies can leverage a Gluu server.  
**Open Source Gluu Server**
- Gluu Server is an open-source software consisting of software written by Gluu and incorporated by other open-source projects. Gluu also supports the open-source as:
The source code is a free open-source with several essential interception scripts written by Gluu to implement the custom business logic for authentication and authorization. 
Gluu server binaries are free for different distributions (packages, containers, K8, snap etc.).

- Gluu Server documentation is freely available. 

- Gluu offers free and VIP support. The Gluu support portal is open to browse, register and post questions. The support portal (https://support.gluu.org) serves as a knowledge 
base for the users. The VIP support comes under a paid contract with guaranteed response time and consultative support (more details: https://www.gluu.org/pricing/). 

**Open Standards behind Gluu Server**

Gluu Server uses several open standards. Subsequent paragraphs discuss a brief detail of these standards.

**OAuth:** OAuth is an open-standard authorization protocol for authorization. It describes the way of authenticated access among unrelated servers. OAuth 2.0 is an industry-standard authorization protocol that focuses on client developer simplicity and providing authentication flows for different applications.

**OpenID Connect:** OpenID connect (OIDC) is an identity layer on top of OAuth 2.0 protocol. OIDC enables the users to verify users’ identity, which was authenticated by an authorization server or identity provider (IDP). OpenID Connect specifies a RESTful HTTP API using JSON as a data format.

**SAML:** Security Assertion Markup Language (SAML) is a standard for Web-based single sign-on (SSO). SAML is an XML-based open-standard for exchanging authentication and authorization between the IDP and service provider (SP).

**UMA:** User-Managed Access (UMA) is an OAuth-based access management protocol standard. The purpose of UMA is to enable the resource owners to control the data sharing and protected resource access among the online services on the owner’s behalf.

**RADIUS:** RADIUS is an AAA (Authentication, Authorization, and Accounting) protocol to manage network access. RADIUS uses two packet types, namely Access-Request for authentication and authorization and Accounting-Request for third A, accounting. 

**SCIM:** System for Cross-Domain Identity Management (SCIM) is a standard to automate the user identity exchange on the web and in cross-domain applications such as enterprise to cloud service providers or inter-cloud services. 

**CAS:** Central Authentication Service is an SSO protocol for web-based applications. CAS protocol mainly involves a client (usually a web browser), a web application requesting the authentication, and a CAS Server.

**Gluu Server Design Goals**
The following are the underlying design goals behind Gluu server development. 

**High Concurrency:**  By design, Gluu Server supports applications with millions of authentication records and billions of authentications in a day.

**High Availability:**  Gluu Server supports the clustering of multiple instances to enable high availability with proper load balancing of the services. 

**Flexibility:**  This is another key design goal behind the Gluu Server is its capability to handle various requirements. The Gluu team has developed several interception scripts to enable the customization without forking the Gluu server code to achieve flexibility.

**Low cost of operation:** The selection of various components enables the lower operating cost of the Gluu Server deployment without compromising the performance and essential design goals (concurrency, availability and flexibility). 

## Common Use-cases
This section presents brief details of the typical use cases of the Gluu Server. 

**SSO:** Single Sign-On (SSO) is an authentication scheme that enables the related yet independent applications to allow users to use a single user id and password for login and access them. This scheme also allows the users to sign in only once to access all the related applications. 

**API Access Management:** API Access Management helps ensure that only authenticated calls will access the APIs. 

**Mobile Authentication:** It is a user identity verification method using a mobile device and at least one more authentication method for secure access. 
IoT: Gluu server efficiently manages the authentication requirement of IoT applications involving a wide range of heterogeneous devices. Gluu Server is suitable for the IoT applications using TCP-based communication. 

**Strong Authentication:** Gluu server supports various strong authentication mechanisms, including U2F keys, OTP apps, and a free and secure mobile push app Super Gluu.  

## Identity Components Gluu Server

**oxAuth:** oxAuth is an open-source OpenID Connect Provider (OP) and UMA Authorization Server (AS). The project also includes OpenID Connect Client code, which can be used by websites to validate tokens.

oxAuth is the web tier of the Gluu Server; it is stateless and can be scaled horizontally. 

oxAuth currently implements all required aspects of the OpenID Connect stack, including an OAuth 2.0 authorization server, Simple Web Discovery, Dynamic Client Registration, JSON Web Tokens, JSON Web Keys, and User Info Endpoint.

**oxTrust:** oxTrust is a Weld based web application for Gluu Server administration. oxTrust enables administrators to manage what information about people is open to partner websites. 

oxTrust is also the local management interface that handles other server instance-specific configurations and provides a mechanism for IT administrators to support people at the organization who are having trouble accessing a website or network resource.

**Passport-js:** Gluu bundles the Passport.js authentication middleware project to support user authentication at external SAML, OAuth, and OpenID Connect providers known as inbound identity.

The integration consists of custom authentication scripts and web pages for oxAuth, and the Passport Node.js application. Together, these assets coordinate the flows and interfaces required to configure the Gluu Server to support inbound identity with a range of external providers.

**Casa:** Gluu Casa ("Casa") is a dashboard for users to manage authentication and authorization data in the Gluu Server. Casa is a self-service web portal for end-users to manage authentication and authorization preferences for their account in a Gluu Server. 

**Oxauth-rp:** The Gluu Server ships with an optional OpenID Connect RP web application called Oxauth-rp, and it is useful for testing. During the Gluu Server setup, the user may opt to install it, and we recommend it for a development environment. Using this tool, you can exercise all of the OpenID Connect APIs, including discovery, client registration, authorization, token, user info, and end_session.

**oxd:** oxd exposes simple, static APIs for web application developers to implement user authentication and authorization against an OAuth 2.0 authorization server like Gluu.

**Super Gluu:** Super Gluu is a push-notification two-factor authentication (2FA) mobile application. Super Gluu uses public-key encryption as specified in the [FIDO U2F authentication standard](https://fidoalliance.org/specifications/overview/). Upon device enrollment, Super Gluu registers its public key against the Gluu Server's FIDO U2F endpoint. When authentication happens, there is a challenge-response to ensure that the device has the corresponding private key.

**Gluu Gateway:** Gluu Gateway (GG) is an authentication and authorization solution for APIs and websites. GG bundles the open-source [Kong Gateway](https://konghq.com/community/) for its core functionality and adds a GUI and custom plugins to enable access management policy enforcement using OAuth, UMA, OpenID Connect and Open Policy Agent (OPA). GG also supports the broader ecosystem of [Kong plugins](https://docs.konghq.com/hub/) to enable API rate limiting, logging, and many other capabilities.

**Cluster Manager:** Cluster Manager (CM) is a GUI tool for installing and managing a highly available, clustered Gluu Server infrastructure on physical servers or Virtual Machines (VMs).  Gluu Cluster Manager does not support CloudNative container based installations such as Docker and Kubernetes.

Cluster Manager can be configured and used to automatically synchronize security updates, configuration changes, user and data object changes, LDAP schema changes, automate key rotation, centrally manage logs, as well as automating the number of other mission critical human error prone  operational support functions. 

## Persistence

Initially, the Gluu Server was using LDAP as the default persistent storage method. From version 4.0, Gluu Server uses the following options for the persistent storage of essential authentication-related information. 

**LDAP:** The Gluu Server uses OpenDJ as its internal directory server to store data generated by the service, such as user profiles, configuration data, tokens, and credentials. This is the default persistence method.

The local LDAP server included in all Gluu Server deployments (i.e. Gluu LDAP) supports multi-master replication (MMR). Any instance can be written to and changes are propagated to other instances.

**Couchbase:** Couchbase is a second object repository method supported by Gluu. Couchbase stores are recommended for high value, high transaction environments. For example, if the number of users are in the hundreds of thousands, but, number of concurrent logins is small,  hundred per minute, LDAP can support such operations. For the ame hundreds of thousands or millions users if the number of concurrent logins goes up to the hundreds per second or more, Couchbase use is strongly recommended.
The Gluu Server supports both local and remote Couchbase clusters. Gluu Server can support a billion authentication per day because of the use of Clouchbase as its database and the cloud-native technologies.

**Hybrid:** It is a meta module that enables the storage of the data to multiple persistence modules. For example, LDAP keeps the user and group entries, while Couchbase stores all other entries. We recommend to avoid the hybrid persistence as it leads to double the maintenance task for both the LDAP and Couchbase. 

**RDBMS:** The latest addition to Gluu Server persistence is RDBMS's support (e.g., Postgress) to store the authentication-related information.  


## Caching

The Gluu Server supports the following four methods for caching of session data and generated tokens. 

**In-memory:** In-memory caching is recommended only for small deployments.

**Redis:** Gluu server supports highly available Redis cache cluster for relatively large and performance-critical deployments.

**Native Persistence (Couchbase, LDAP):** This caching method uses the underlying native persistence method of the Gluu Server deployment. If the LDAP/ Couchbase is the database of any deployment, then under this scheme, LDAP/ Caouchbase stores the caching data.

## Suggested Deployment Architecture

The actual Gluu Server deployment depends on the application. Several factors (size of the application in terms of number of users, number of authentication per day, etc.) affect the deployment architecture. Mostly the applications can be efficiently served by single server deployment. There may be some mission-critical and large application that requires the highly available multiple-server/ cluster-based Gluu Server deployment. 

**Single Server Deployment:** There are several ways a Gluu Server is deployed on a single server instance as discussed in subsequent paragraphs.

- **Single Server VM / Linux Packages chroot distribution:** A user can install the Gluu Server inside a VM of physical servers with at least 2 CPU units and 4 GB RAM. The Gluu Server is a Chroot container. Chroot is a UNIX operation that changes the root directory of the current running process and its child processes. The running process under this modified environment cannot access the files outside the assigned directory tree. Chrooted environment provides file system isolation to the processes but is different than the container environment like LXC or Docker as it shares other resources of host for example IP address. 

**Single Server VM / Linux -nochroot distribution:** Beginning with version 4.2.1, Gluu server Community Edition is available for install directly to the operating system without the use of the chroot container. Same hardware specifications are used for the nochroot install.

- **Snap Package:** A Canonical product named as Snap is a software packaging and deployment system for the Linux kernel-based operating systems. The snap package for Gluu-ce is named as gluu-snap. This package will have the same structure like the RPM or DEB of Gluu-ce. 
Cloud-based Gluu Server Deployment: Gluu server supports most of the cloud providers, a few selected providers are as:

- **Digital Ocean:** Gluu Server can be deployed on the digital ocean cloud using containership.io. Containership is a containers-as-a-service platform that makes it easy to deploy, manage and scale containerized applications on any public or private cloud.

- **Amazon AWS:** Gluu Server also supports installation on Amazon AWS instances with the private address. 

## Cluster Deployment

Gluu Server supports various active-active high availability deployment models.

**Manual:** A difficult to configure choice for a high available Gluu Server cluster across multiple instances is to configure it manually. The cluster requires a minimum of four servers or VMs as two for Gluu Server, one for load balancing and one for redis. This requires a lot of effort towards making sure that the deployment meets the performance requirement. The individual components must be properly tested and benchmarked for the performance requirement. Note*. Manual cluster configuration is not eligible for Gluu support. 

**Cluster Manager:** Cluster Manager (CM) is a GUI tool for installing and managing a highly available, clustered Gluu Server infrastructure on physical servers or VMs. CM can be used to cluster an existing single node Gluu Server, or can be used to deploy a new cluster of Gluu Servers from scratch. CM automates several tasks associated with setting up and managing the high available cluster including its installation, database replication, cache management, logging, secure tunneling between oxAuth and redis. 

**Docker-Kubernetes Cloud Native Implementation:** Kubernetes is an open source container-orchestration system to automatically deploy, scall and manage the application. Gluu Server supports configuration of both cloud (amazon’s EKS, Google’s GKE, DigitalOcean’s DOKS, Microsoft AKS etc.) and local kubernetes (Minikubes, MicroK8s) clusters. 

## Sizing
Gluu Server supports applications of various sizes from a few hundreds/ thousands of authentications per day to a billion authentications per day. There are several parameters that affects the sizing decisions, a carefull consideration of these parameters helps in taking selecting aprorpiate sizing for the organization/ applications. This section will present an overview and in-depth discussion of these parameters covering how they affect the Gluu server's deployment and performance. These parameters are the total number of users (active & idle), desired authentication throughput, type of authentication application workflow, preferred persistence method (database and caching) technique, desired redundancy model, and the deployment stack like VM/ physical server-based to Cloud-native and Kubernetes based deployment.

The following table presents details of these parameters and their impact on the performance.

|Parameter        |It's Impact              |
|-----            |-----------              |
|Number of Users      |As the database size grows with the users as active and inactive users, occupy space. The number of active users also affects the caching size, and ultimately it also plays an important role in the physical memory size.|
|Desired Authentication Throughput|The desired authentication throughput is an essential parameter to take the sizing decisions as Gluu Server supports low, medium, high to extremely high authentication throughput. Gluu Server deployment on a single VM/ server will meet the low and medium authentication throughput demands. For extremely large (a billion authentication per day), we recommend Gluu Server deployment with Couchbase, Cloud-Native, and Kubernetes.|
|Application Workflow    |The application flow also affects the sizing decision as a single Gluu Server instance (VM/ Physical Server) can efficiently handle the demand of the applications that require simple authentication token generation. For complex application workflows that may require multistep authentication, the involvement of AI for fraud detection, etc. one could think of more sophisticated deployment involving cluster, caching, and efficient persistence method.|  
|Preferred Persistence Method     |Gluu Server supports different persistence methods to cope up with various applications' demands. It includes several combinations of different databases and caching to support applications of various scales.|
|Desired Redundancy Level        |The opted redundancy model obviously affects the size of deployment in terms of the number of instances.|
Preferred Deployment Stack        |The different Gluu Server deployment stacks have their own requirement for the number of instances & configuration of each instance. Accordingly, they support a different level of performance.| 

- **VM:**

- **Kubernetes:**
   - **Couchbase:**
   - **LDAP/ Redis:**
