PZP Discovery
=============

{{toc}}

1. Introduction
---------------

webinos PZP discovery defines interfaces and procedures to allow automatic detection of PZPs in other devices, within or outside the own personal zone, via local or remote connections.

PZP discovery plays an essential role for direct PZP to PZP communications. With the personal zone concept in mind, it offers PZP the freedom of accessing services that reside in other PZPs via secure connections while its own PZH is not reachable or prefers to connect to other PZP directly rather than through PZHs.

From webinos discovery API point of view, PZP is treated as a service of webinos. It is, however, different from other webinos services. PZP provides the functionality of hosting and managing other webinos services.

PZP discovery MAY be trigged by a request of finding other PZPs or by a request of finding a Webinos service hosted in PZP. In the latter case, when the request is initiated without knowledge of what PZPs are available, the service discovery procedure involves two major steps: to find other PZPs and to find webinos services provided by the found PZPs. This specification only covers PZP discovery. For details on finding webinos services provided by certain PZPs, please refer [[Service_Discovery]] specification.

Facing the challenges of a wide variety of discovery protocols used these days due to multiplicity of networking technology and various working scenarios introduced by webinos personal zone concept, webinos PZP discovery aims at providing a generic PZP discovery interface for web developers in all circumstances.

This specification outlines PZP discovery process in three steps

-   PZP registration and advertisement
-   Querying or finding other PZPs based on certain criteria
-   Binding with discovered PZPs

Descriptions of each of these steps are associated with two discovery approaches based on network availability:
* PZH assisted PZP discovery. This requires the availability of network connections between PZPs and their PZHs, and between PZHs if inter-zone PZP discovery happens.
* Direct PZP discovery. It refers a direct discovery conversation between PZPs. Various existing discovery mechanisms, e.g. Zeroconf, UPnP, SLP, and XMPP extension on service discovery etc, can be used for this purpose, either via local or remote network connections.

When existing discovery mechanisms are adopted for PZP discovery, a protocol wrapper/mapper is required to isolate the complexity of different internetworking technologies and to present discovery interfaces as generic as possible to the web developers. In this sense, the discovery operations are triggered by webinos PZP discovery interfaces at high level, and executed by the low level discovery schemes. The relationship among these three parties is shown in the following diagram.

![](http://dev.webinos.org/redmine/attachments/2289/wrapper.png)

In the following sections, the existing discovery schemes are referred as low level discovery schemes.

2. Technical Formal Specification
---------------------------------

For each working scenario described in this section, it is assumed that:

-   Each PZP has enrolled in a personal zone. [[PZP_Discovery#33-PZPEnrolment|PZP Enrolment]] is defined by when a PZP receives a certificate signed by the PZH for all future TLS connections. The enrolment of a PZP is part of PZP registration process and details are described in 2.1.1.
-   Zones have authenticated each other in inter-zone PZP discovery scenario. To ensure a secure and successful inter-zone discovery, e.g. a PZP in zone PZ_A to discovery PZPs in zone PZ_B, zones involved in the discovery process have to authenticate with each other beforehand. It means that both parties have registered each other’s certificate as being trusted after a certificate exchange procedure ([[PZP_Discovery#33-Certificate Exchange between Users | Certificate Exchange between Users]]).

### 2.1. PZP registration and advertisement

One of the key areas in PZP discovery is to make the PZP discoverable or visible to potential PZPs that perform PZP searches. This requires registering and advertising the PZP at a point, e.g. a database in a central server or service list file in a local device that others can locate or look up.

By default, the PZH in a personal zone holds a database that lists information of PZPs in the zone. In the case that PZH is not available or direct PZP discovery is preferred, each device that has PZP service SHOULD provide a service list file locally that has descriptions of PZP service. This allows under level discovery scheme to look up for available services in the local device. For example, in avahi (Linux implementation of Zeroconf), an .xml file stored locally lists all the services provided by the device that are discoverable to others. When a discovery request is received by this device, it looks up this .xml file and check whether the requested service is in the list.

#### 2.1.1 PZP registers with PZH

In webinos, the first time PZP connects to its PZH, it shall register its service information to the PZH. The registration involves auto enrolment and TLS connection setup between PZP and PZH. During this process, PZP SHOULD provide identity information to its PZH. The identity includes PZP network address and the PZP Server TLS port. See [[PZH_Admin#Device-Enrollment|PZH Admin specification]] for more details for this procedure

This information is updated under following circumstances:

-   On receiving change notification
-   On PZP connecting or disconnecting to its PZH

PZH has provided a central place to maintain the information that all its registered PZPs provide to it. The following diagram shows the message flows when PZPs registers with their PZH.

<div class="uml">

PZP_A -> PZH : setupTLSConnection(PZP_A_ID)
 PZP_A -> PZP_A : openListenerChannel
 PZH -> PZH : addPZPInfo(ConnectedPZPList, PZP_A_info_object)

PZP_B -> PZH : setupTLSConnection(PZP_B_ID)
 PZP_B -> PZP_B : openListenerChannel
 PZH -> PZH : addPZPInfo(ConnectedPZPList, PZP_B_info_object)

</div>
#### 2.1.2 Register PZP in existing discovery schemes

In the case that PZH is not reachable or direct PZP discovery is preferred, PZP relies on low level discovery schemes, for the purpose. To be able to use low level discovery schemes for PZP discovery, the PZP service MUST make itself registered and available in the service list or service database of the discovery scheme. Registration of new service in low level discovery scheme is triggered by a call on the generic PZP registration interface. The registration interface SHOULD provide options to register PZP as a service to low level discovery scheme on demand or on the fly.

Although each specific discovery scheme defines its own way to handle new service registration, most widely used discovery schemes these days follow the same or similar service registration procedures. A PZP registration procedure SHOULD generalize these steps as shown in the following diagram:

![](http://dev.webinos.org/redmine/attachments/2279/PZP_registration.png)

#### 2.1.3 Service type mapping

To register with low level discovery schemes, PZP registration interface MUST at least specify the PZP service type. The service type attribute is described as follows:

  --------------- --------------------------------------- -------------- ---------------
  **Attribute**   **Description**                         **Required**   **Data type**
  ServiceType     URI used to identify PZP service type   mandatory      String
  --------------- --------------------------------------- -------------- ---------------

URI string used to identify PZP service type SHOULD be:

http://webinos.org/pzp

URI string format is not adopted by all low level discovery schemes. While registering or creating PZP service in these schemes, a type mapping scheme SHOULD be provided in PZP discovery design. The following diagram gives an example on how this works.

![](service_type_mapping.png)

### 2.2 Finding other PZPs

#### 2.2.1 Attributes and Interfaces

Finding PZPs operation is triggered by webinos JavaScript [[PZP_Discovery#32-DiscoveryAPI|discovery API]]. The finding process varies depending on working scenarios and criteria specified by the operation. The following attributes and interfaces are specified for finding PZPs operation to accommodate possible searching criteria:

  ------------------------ --------------------------------------------------------------------------------------------------------------- -------------- --------------- -- ----------------- -------------------------------------------- ---------- ----------------
  **Attribute**            **Description**                                                                                                 **Required**   **Data type**
  ServiceType(API)         URI used to identify PZP service type.                                                                          mandatory      String
  FindCallBack             Asynchronous callback used whenever a new PZP is found                                                          mandatory      Interface
  Filter:zoneId            Identities of personal zones                                                                                    optional       String
  Filter:remoteServices    If remoteServices is true, the find PZPs method will extend the search for PZPs outside the local IP network.   optional       boolean
  Filter:DiscoveryMethod   Discovery methods to be used                                                                                    optional       String
  ------------------------ --------------------------------------------------------------------------------------------------------------- -------------- --------------- -- ----------------- -------------------------------------------- ---------- ----------------

-   **ServiceType**

Refer 2.1.3 Service type mapping section above for more details.

-   **FindCallBack**

The search query is asynchronous and a success callback onFoundCallback is issued for every PZP that is found.

To monitor the status of each found PZP, the onLostCallback is provided to handle the case when a found PZP is not available for binding in the later stage of discovery.

Error Conditions are also handled via callbacks. The error types defined shall be generic isolating from the complexity of low level discovery schemes used. Error types follow definition of Errors in ([[PZP_Discovery#31-DOM4Errors|DOM4Errors]]). Error types for this operation are illustrated but SHOULD not be restricted by the list below:

  -------------------- --------------------------------------------------------------------- -- --------------- --------------------------------------
  Error Type           Causes
  TypeMismatchError    Wrong of PZP service type
  TimeoutError         If timeout Option is used and time out happens
  InvalidAccessError   Policy restriction set by webinos system.
  NotSupportedError    The Filter fields specified, e.g. DiscoveryMethod, is not supported
  AbortError           The discovery process was cancelled by the application
  -------------------- --------------------------------------------------------------------- -- --------------- --------------------------------------

-   **Options:Timeout**

Option to set time frame for search process. The time for finding services can vary significantly depending of which type of discovery method that is used. The timeout value shall be set in conjunction with system settings, working scenario and discovery method used.

-   **Filter**

DiscoveryMethod is specified for direct PZP discovery scenario. The use of DiscoveryMethod has tight coupling relationship with remoteServices filter field. Whilst remoteServices is disabled, local discovery methods, e.g. zeroconf, Bluetooth SDP etc, apply. If remoteServices is enabled, remote discovery mechanisms, e.g. XMPP discovery, apply.

In the case that zoneId is not specified, PZP discovery shall only look for other PZPs in the same zone.

In the case that DiscoveryMethod is not specified, PZP initiates PZH-assisted discovery first. Should it fail, a default DiscoveryMethod set by the system shall be used.

The finding PZPs operation SHOULD continue to search for all services that match the searching criteria unless the operation is cancelled by the application or when the maximum search timer expires.

#### 2.2.2 Finding other PZPs with the assistance of PZH

The PZH-assisted PZP search procedure is described via message sequence diagrams in this section. Both inter-zone and intra-zone PZP discovery scenarios are covered.

**2.2.2.1 Finding PZPs in Own Personal Zone**

It is assumed that the PZP that performs searching operation is connected with its PZH. Since PZH holds the information of each PZP registered with it in the zone, it will pass this information to any connected PZP that searches for other PZPs in the zone. PZH is responsible for managing and updating information on each PZP’s status of availability. If a found PZP’s status is unavailable, an onLost callback is initiated at the PZP that starts the searching process. Otherwise, a onFound callback is called. Error conditions are handled using onError callbacks.

The message flows between PZP and PZH directly. When PZH identify that the findservices request is on finding PZPs rather than other webinos services resides in a PZP, it hanles the message directly rather than passing it to RPC handler. This request a sepcific handler functioin on PZH side for database lookup and return result to PZP that initiates discovery request.

<div class="uml">

PZP -> PZH : FindServices(ServiceType:PZP)

group onFound
 PZH -> PZP : onFound(Service:ConnectedPZPList)
 PZP -> PZP : updatePZPInfo(ConnectedPZPList)
 end

group onLost
 PZH -> PZP : onLost(Service:PZP)
 PZP -> PZP : updatePZPInfo(PZP)
 end

group onError
 PZH -> PZP : onError(Error:ErrorType)
 PZP -> PZP : ErrorHander(ErrorType)
 end

</div>
**2.2.2.1 Find PZPs in other people’s personal zone**

In the following diagram, PZP_A is the party that initiates the PZP discovery process. PZP_A’s PZH is PZH_A. The two PZHs, PZH_A and PZH_B, have authenticated with each other, which means that both parties have registered each other’s certificate as being trusted after a certificate exchange procedure. Since each PZH holds information about the PZPs within its zone, a query on PZPs in a zone of a different user is directed to the PZH of that zone.

<div class="uml">

PZP_A -> PZH_A: FindServices(ServiceType:PZP, Filter{ZoneId:PZ_B})
 PZH_A -> PZH_B: FindServices(ServiceType:PZP, Filter{ZoneId:PZ_B})

group onFound
 PZH_B -> PZH_A : onFound(Service:ConnectedPZPListofPZ_B)
 PZH_A -> PZP_A : onFound(Service:ConnectedPZPListofPZ_B)
 PZP_A -> PZP_A : updatePZPInfo(ConnectedPZPListofPZ_B)
 end

group onLost
 PZH_B -> PZH_A : onLost(Service:PZP)
 PZH_A -> PZP_A : onLost(Service:PZP)
 PZP_A -> PZP_A : updatePZPInfo(PZP)
 end

group onError
 PZH_B -> PZH_A : onError(Error:ErrorType)
 PZH_A -> PZP_A : onError(Error:ErrorType)
 PZP_A -> PZP_A : ErrorHander(ErrorType)
 end

</div>
#### 2.2.3 Finding other PZPs without the assistance of PZHs

In this specification, the procedure of finding other PZPs without assistance of PZHs is referred as direct PZP discovery. Direct PZP discovery MAY happen in both intra-zone and inter-zone scenarios. For inter-zone direct PZP discovery, the assumptions made in the beginning of section 2, which are each PZP has enrolled in a personal zone and zones have authenticated each other, apply. With these assumptions, even in the absence of PZHs, PZPs cross both zones consider each other to be trustable.

**2.2.3.1 Mapping to low level discovery scheme**

Direct PZP discovery relies on low level discovery schemes for the search process. Application calls the high level finding PZPs JavaScript [[PZP_Discovery#32-DiscoveryAPI|discovery API]]. This triggers the discovery execution in low level discovery schemes. In a sense, the real discovery happens in low level while PZP JavaScript API only provides an API interface to the developers. Relationships between the finding PZPs JavaScript API and the low level discovery schemes is summarized in the following diagram.

![](DM_API_relationship.png)

Between the finding PZP JavaScript API and libraries/APIs from existing discovery methods, an interface wrapper is essential. The wrapper SHOULD have the following functionality:

-   Map or wrap PZP service type and other attributes or parameters provides by finding PZP JavaScript API into the attributes or parameters that are recognized by low level discovery scheme.
-   Map or wrap PZP interfaces or function APIs provides by finding PZP JavaScript discovery API into interfaces or function APIs that are recognized by low level discovery scheme.
-   Map or Wrap and present the discovery results produced by low level discovery scheme into the format that finding PZP JavaScript discovery API can recognize.

The interface of finding PZPs with specified attributes or parameters is passed to the wrapper. When DiscoveryMethod is specified, the wrapper will execute the service type and finding PZPs function mappings to the correspondent service type and functions in the low level discovery scheme specified. Otherwise, it looks up all available low level discovery schemes and performs service type and function mapping to each of the schemes.

The following diagram illustrates how the wrapper works when mapping finding PZP JavaScript API into interfaces provided by low level discovery scheme. DiscoveryMethod has been specified as Zeroconf in this example.

![](Function_mapping.png)

In the case that APIs from the low level discovery scheme specified do not have all functionalities that finding PZPs requires, extensions on this discovery scheme MUST be provided for this purpose.

Design of the wrapper MUST be generic and open. Being generic means that for Web developers who do not want to know the details of the bearers can use the generic structures for different discovery mechanisms. Being open means that it provides options for third party developers who would like to implement interfaces have APIs on the internetworking technologies they are interested.

Due to the fact that most of nowadays libraries from existing discovery methods are not exposed as JavaScript interfaces but using native code, e.g. C/C++, Java etc, a language binder is also required in implementations whereas necessary.

**2.2.3.2 Message sequences**

A PZP can only find other PZPs within the same zone and those PZPs with whom their PZHs have authenticated with each other, which means that both PZHs have registered each other’s certificate as being trusted after a certificate exchange procedure.

The following message diagram describes message flow at high level and only covers the success case. For onLost and onError cases, similar handling procedures are followed as described in PZH-assisted intra-zone PZP discovery scenario above.

In this diagram, PZP_A is the party that initiates the PZP discovery process. PZP_B and PZP_C are the PZPs that to be found or found in the specified ZoneId.

<div class="uml">

PZP_A -> Network: FindServices(ServiceType:PZP, Filter:ZoneId)
 Network -> PZP_B : FindServices(ServiceType:PZP, Filter:ZoneId)
 Network -> PZP_C : FindServices(ServiceType:PZP, Filter:ZoneId)
 PZP_B -> Network : onFound(Service: PZP_B)
 PZP_C -> Network : onFound(Service: PZP_C)
 Network -> PZP_A : onFound(Service: PZP_B)
 PZP_A -> PZP_A : addPZPInfo(ConnectedPZPListofZoneId, PZP_B)
 Network -> PZP_A : onFound(Service: PZP_C)
 PZP_A -> PZP_A : addPZPInfo(ConnectedPZPListofZoneId, PZP_C)

</div>
### 2.3 Binding with discovered PZPs

#### 2.3.1 Attributes and Interfaces

The minimum attributes and interface for binding operation SHOULD include:

  --------------- -------------------------------------------------------------------------------------------------------- -------------- ---------------
  **Attribute**   **Description**                                                                                          **Required**   **Data type**
  BindCallBack    Asynchronous callback to report the states of the bind operation and the availability of the found PZP   mandatory      Interface
  PZPId           Unique identity of the PZP for binding                                                                   mandatory      String
  --------------- -------------------------------------------------------------------------------------------------------- -------------- ---------------

-   PZPId

Unique identity of the PZP to be bound. Once a PZP is found the finding PZPs API will return a found PZP object, which SHOULD contain the unique identity of the found PZP. In some cases, PZP identity MAY be obtained without invoking finding PZPs operation, for example, PZH broadcasts the identity of a new registered PZP to all its PZPs.

The PZP identity MAY be presented in different forms depending on implementation environment, e.g. device address, network address or even a randomly generated serial number.

-   BindCallBack

An onBind callback is issued when binding operation is successful.

PZP binding is valid until unbind callback is issued. The binding to the PZP MAY be resumed later by calling onBind again.

To monitor the status of each bound PZP, callbacks SHOULD be provided to handle the case when a found PZP is switching between available and unavailable status.

Error conditions SHOULD also be handled via callbacks. The error types defined SHOULD be generic isolating from the complexity of under layer discovery schemes used. Error types also follow definitions of Errors in ([[PZP_Discovery#31-DOM4Errors|DOM4Errors]]). Error types are illustrated but SHOULD NOT be restricted by the list below:

  ------------------- --------------------------------------------------- -- -------------------- -------------------------------------------
  Error Type          Causes
  InvalidStateError   The bind PZP object is in an invalid state
  SecurityError       If not authorized to bind to the found PZP
  AbortError          The bind process was cancelled by the application
  ------------------- --------------------------------------------------- -- -------------------- -------------------------------------------

#### 2.3.2 Binding Procedure

The starting point for a binding procedure is to obtain the identity of the PZP to bind, which MAY be achieved via different approaches based on implementation environments. In the case that finding PZPs interface is used as described above, each found PZP object returned from finding PZPs procedure SHOULD be stored in a list in an asynchronous manner at the PZP that initiated the search procedure. Once a found PZP object is selected from the list, a PZP binding procedure starts.

Binding to a found PZP is subject to the availability of the PZP. The operation SHOULD not proceed further once a found PZP is unavailable for binding.

Binding to a found PZP is also subject to policy and security enforcements. The bind operation will mutually authenticate the PZPs and verify access/privacy policies. Once binding operation is authenticated, authorized and permitted, the two PZPs involved MAY start establish a communication connection.

3 References
------------

### 3.1 DOM4Errors

http://dvcs.w3.org/hg/domcore/raw-file/default/Overview.html#errors

### 3.2 discovery API

http://dev.webinos.org/specifications/new/servicediscovery.html

### 3.3 PZP Enrolment

http://dev.webinos.org/redmine/projects/wp4/wiki/PZP_Device_Enrolment

### 3.4 Certificate Exchange between Users

http://dev.webinos.org/redmine/projects/wp4/wiki/Certificate_exchange_between_users

