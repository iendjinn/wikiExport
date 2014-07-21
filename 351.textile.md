WP3 Wiki Landing Page
=====================

From the webinos description of work:
> WP3 aims to develop a robust platform independent specification of the webinos framework. Server- and
> network-side as well as device API specifications will be developed based on the use cases and requirements
> which were identified in WP02. Furthermore, common webinos system components for the server and client
> side as well as concepts for the applied security system will be identified and specified. The WP3 activities will
> create specifications that will provide sufficient information to develop and implement webinos runtimes, i.e. the
> distributed webinos platform, and corresponding APIs to the webinos applications. Beside external publication
> these specifications will be provided to WP4, which will implement the webinos system for the four target
> domains (mobile, home media, PC, automotive).

WP3 is the work package concerned with specification. It specifies not only the API and runtime behavior on individual devices, but also the required execution environment (such as external services and components that need to be accessible, but are not necessarily located on the device itself) and the security framework (which, similary, also not only covers the security on an individual device, but also the security of the device environment).

The basic approach is a (slightly modified) two phase waterfall model. The specification is based on the requirements, use cases and scenarios from WP 2 and will be implemented in WP4. In the second phase of the project, based on the experience gained from the implementation and application development work, as well as incorporating changes in technology and the market place, the specification will be updated and extended.

At the moment, WP3 has finished phase I and is formally on a hiatus. There is, however, a close cooperation with the implementation workpackage (WP4) and any clarifications to the specifications, which appear do to implementation experience, will be reflected in the specifications.

Tasks
-----

Formally, WP3 consists of five tasks. But due to the two phase approach of webinos, two of the tasks (namely 3.3 and 3.4) cover the same areas as two other tasks (3.1 and 3.2), only about 14 months later, so that the tasks only cover three different areas:

### Task 3.1 (and Task 3.3 for phase II)

This task specifies the architecture and required infrastructure and service components for the webinos project.

The primary areas covered in this task are the webinos foundations, extension handling, authentication, discovery, messaging, context, security, privileged applications and analytics.

These topics are supplemented with a component overview, a high level network overlay architecture, as well as session and synchronisation handling decriptions.

### Task 3.2 (and Task 3.4 for phase II)

This task covers the APIs to be implemented by a webinos device.

According to the use cases and requirements defined in WP2 and the common components introduced in task 3.1, task 3.2 defines a set of application programming interface specifications (APIs) to make the desired functionality available to webinos applications.

The webinos APIs can be divided into a number of categories:
* webinos base and generic objects/interfaces: For example the webinos core interface
* service discovery and remote API access APIs
* HW Resource APIs (such as GPS, camera, microphone, sensors)
* Application Data APIs (such as contacts, calender information, messages)
* Communication APIs
* Application execution APIs (allowing webinos applications to launch other webinos and native applications)
* User profile and context APIs
* Security and Privacy APIs

### Task 3.5 (for phase I and phase II)

The security and privacy architecture aims to protect webinos users and systems from many threats, including those of malicious software, unauthorised data collection, violations of privacy and loss of personal data.
In this task existing mobile security architectures are analysed, key threats are identified, several pieces of security and privacy-protecting functionality are specified and guidelines are provided to developers of the webinos runtime. Security functionality includes a security and privacy policy architecture, platform integrity checking, authentication, authorisation, and interfaces to manage the end user’s new personal webinos network of devices.

Status
------

WP3 has completed its first phase at the end of June 2011.

The information about the areas covered in WP3 so far can be found here:
"T3.1 System Specification":http://dev.webinos.org/redmine/projects/wp3-1/wiki
"T3.2 Device API Specification":http://dev.webinos.org/redmine/projects/t3-2/wiki
"T3.5 Security Framework":http://dev.webinos.org/redmine/projects/t3-5/wiki

While ‘emergency fixes’ and clarifications will lead to updates to the specifications, WP3 is now essentially on a hiatus until spring 2012, when the WP will enter its second phase.

Available Deliverables
----------------------

So far WP 3 has produced three deliverables for submission to the EU on July, 1st 2011:
"Deliverable 3.1 System Specification":http://dev.webinos.org/redmine/projects/wp3-1/wiki/WP_31_Deliverable
"Deliverable 3.2 Device API Specification":http://dev.webinos.org/redmine/projects/t3-2/wiki/WP_32_Deliverable
"Deliverable 3.5 Security Framework":http://dev.webinos.org/redmine/projects/t3-5/wiki/Deliverable_Outline

These are the deliverables covering the specification for phase I on the project. There will be an update for each of the deliverables after phase II of webinos. These deliverables will be available at the end of August 2012.

Communication Resource
----------------------

Communication within the WP is mainly done on this Wiki and the WP3 specific mailing list:
webinos-wp3-ml@fokus.fraunhofer.de
Access to the mailing list (reading as well as sending) is currently for webinos members and associated partners only.
To be added to the mailing list, please contact the mailing list administrator "Christian Fuhrhop":http://dev.webinos.org/redmine/users/36

Responsibilities
----------------

  ----------------- --------------------------------------------------------------- ---------------------------------------------------------------------------------- ---------------------- -- ---------------- ------------------------------------------------------------- --------------------------------------------------------------------------------- ------------------
  **Role**          **Person**                                                      **Bio**                                                                            **Company**               Project Leader   "Stephan Steglich":http://dev.webinos.org/redmine/users/9   "Bio":http://dev.webinos.org/redmine/projects/wp-0/wiki/Stephan_Steglich   Fraunhofer FOKUS
  WP 3 Leader       "Christian Fuhrhop":http://dev.webinos.org/redmine/users/36   "Bio":http://dev.webinos.org/redmine/projects/wp-0/wiki/Christian_Fuhrhop   Fraunhofer FOKUS
  Task 3.1 Leader   "Christian Fuhrhop":http://dev.webinos.org/redmine/users/36   "Bio":http://dev.webinos.org/redmine/projects/wp-0/wiki/Christian_Fuhrhop   Fraunhofer FOKUS
  Task 3.2 Leader   "Claes Nilsson":http://dev.webinos.org/redmine/users/52       "Bio":http://dev.webinos.org/redmine/projects/wp-0/wiki/Claes_Nilsson       Sony Ericsson Mobile
  Task 3.3 Leader   "Alan Baldwin":http://dev.webinos.org/redmine/users/31        "Bio":http://dev.webinos.org/redmine/projects/wp-0/wiki/Alan_Baldwin        Samsung
  Task 3.4 Leader   "Claes Nilsson":http://dev.webinos.org/redmine/users/52       "Bio":http://dev.webinos.org/redmine/projects/wp-0/wiki/Claes_Nilsson       Sony Ericsson Mobile
  Task 3.5 Leader   "Andrew Martin":http://dev.webinos.org/redmine/users/22       "Bio":http://www.softeng.ox.ac.uk/Andrew.Martin/bio.html                         University of Oxford
  ----------------- --------------------------------------------------------------- ---------------------------------------------------------------------------------- ---------------------- -- ---------------- ------------------------------------------------------------- --------------------------------------------------------------------------------- ------------------


