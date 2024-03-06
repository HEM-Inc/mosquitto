
# Mosquitto Password file generation using github secrets

# Functional Specification Document (FSD)

## Table of Contents
* [Revision](#revision)
* [Feature Overview](#1-feature-overview)
	* [Requirements](#11-requirements)
		* [Functional Requirements](#111-functional-requirements)
		* [Configuration and Management Requirements](#112-configuration-and-management-requirements)
		* [Scalability Requirements](#113-scalability-requirements)
		* [Restart Requirements](#114-restart-requirements)
		* [Error Handling](#115-error-handling)
* [Functionality](#2-functionality)
	* [Functional Description](#21-functional-description)
* [Design](#3-design)
	* [Overview](#31-overview)
	* [Config Changes](#32-config-changes)
* [Flow Diagrams](#4-flow-diagrams)
* [Serviceability and Debug](#5-serviceability-and-debug)
* [Restart Support](#6-restart-support)
* [Restrictions/Limitations](#7-restrictionslimitations)
* [Test plan](#8-test-plan)
	* [Test cases](#81-test-cases)
	* [Integration test cases](#82-integration-test-cases)



### Revision
| Rev |     Date    |       Author                | Change Description                |
|:---:|:-----------:|:-------------------------:|:-----------------------------------:|
| 0.1 |  02/22/2024           |     Prateeksha Shanbhogue       |    Initial version  |

## **1. Feature Overview**

Hemsaw makes use of Mosquitto MQTT broker which is a light weight publish/subscribe messaging model.
The MTConnect agent publishes the data to the MQTT broker and the broker publishes these data to the subscribed end clients or application.

All clients of MQTT broker(including mtconnect agent) can get connected to the broker with or without authentication(login username and password).

Mosquitto broker will generate an encrypted password file for every user.Encrypted password file needs to be generated using mosquitto command line utility.

Github actions provides utility to securely store these username and password as secrets.These github secrets can be used by applications to generate passwords.

Encrypted password file generation can be automated, using github secret during the dockerization of MQTT broker. Such generated password will be used by mosquitto for client authentication.

### **1.1 Requirements**
#### 1.1.1 Functional Requirements

1. To automate the mosquitto password file generation using github secrets and docker secrets.

2. To make the github actions run and push the docker image to the docker-hub using the secrets generated for the username and password in the github secrets

    - The secrets generated shall be used as environment variable in the github workflow, so that it will be available in the dockerfile for password file generation
    - Generated password  shall be used for user authentication.


#### 1.1.2 Configuration and Management Requirements
The password generated using the github secret functionality shall be stored in the password file as specified in mosquitto configuration file. Configuration of Password file path is an already existing feature in the mosquitto and same configured path shall be used to store the generated passwords. 


#### 1.1.3 Scalability Requirements 
There can be upto 100 github secrets, as specified by github documentation.

#### 1.1.4 Restart Requirements
* Upon changing the secrets in github, it is required to rebuild the docker image and restart the mosquitto docker image using ssUpgrade script. 
* As per existing behaviour, any modifications in the mosquitto configuration which effects the password generation will need re-running the ssUpgrade script with a *-M* option representing mosquitto update

#### 1.1.5 Error Handling 
* When an unauthorised user tries to connect to the MQTT broker with invalid credentials(username and password) the broker rejects the connection and logs the error.

## **2. Functionality**
### **2.1 Functional Description**
* This feature will generate the password file whithout needing to be mounted manually while running the docker container.
* The dockerfile takes the secrets stored in the github as environment variables and runs the mosquitto password file generation command which inturn generates the password file inside the docker image.



## **3. Design**
### **3.1 Overview**
The design strategy involves utilizing GitHub Actions, a feature provided by GitHub for automating workflows, along with secret management, to create password files for Mosquitto MQTT

Docker secret syntax to mount the github environment variable to the docker container:

    RUN --mount=type=secret,id=SECRET_ID,target=/run/secrets/PASSWORD_FILE_NAME


### **3.2 Config changes**
* It is recommended to have mosquitto config file with *allow_anonymous* attribute set to false, so that only authorised users can connect to the broker.
* Paths to password file to be mentioned in mosquitto config file.

## **4. Flow Diagrams**
Not applicable

## **5. Serviceability and debug**
Not Applicable

## **6. scalabiity**
Not Applicable

## **7. Restrictions/Limitations**
* Upon the completion of the image construction and its availability for utilization, any alteration to the password necessitates the rebuilding of the image.


* If the username and password are invalid, the user will not be able to connect to the broker

* If the  *allow_anonymous* attribute is set to true, then even the users with invalid username and password will be able to connect to the broker

## **8. Test plan**

### **8.1 Test Cases**

| Test   Case ID | Test   Category                | Test Case   Description                                       | Expected Result                                             | Status | Comments |
|----------------|-------------------------------------|---------------------------------------------------------------|-------------------------------------------------------------|--------|----------|
|1              | Secret generation Using github      | Verify username and passwords   creation using github secrets |  Username and password creation successful                  |        |          |
| 2              |                                     | Verify the updated secrets in github                          | Updation successful                                         |        |          |
| 3             | Docker build using github actions | Verify valid github secrets are   mount to the container      | Image Successfully  pushed to docker-hub                    |        |          |
| 4            |                                     | Verify invalid github secret ID mount to the container    | Error in github actions and no   image pushed to docker-hub |        |          |
| 5             |                                     | Verify docker image build                                     | Image push to docker-hub   successful                       |        |          |
| 6               |                                     | Verify use invalid   environment  variable while building     | Error and no image pushed to   docker-hub                   |        |          |
|7              |   | Verify trigger in github workflow on push |  Trigger successful
|                  |        |          |                |                                     |                                                               |                                                             |        |          |


### **8.2 Integration Test Cases**
| Test Case ID | Test Case Description                                                            | Expected Result                                  | Status | Comments |   |
|--------------|----------------------------------------------------------------------------------|--------------------------------------------------|--------|----------|---|
| 1            | Verify mosquitto image pull in  IPC                                         | Mosquitto image run successfull           |        |          |   |
| 2            | Verify the integration of the mosquitto image with agent                               | Logs received in agent and broker                                   |        |          |   |
| 3            | Verify data received from agent                     | Mosquitto run Success                            |        |          |   |
| 4            | Check with invalid port configuration  in mosquitto.conf                                                 | Error in mosquitto run                              |        |          |   |
| 5            | Verify broker connection with invalid  username or password when allow_anonymous is false              | Authentication error in broker                           |        |          |   |
| 6            | Verify broker connection with invalid  username or password when allow_anonymous is true               | Connection successful                            |        |          |   |
| 7            | Verify broker connection for anonymous user with allow_anonymous is false | Authentication error in broker |        |          |   |
| 8            | Verify broker connection  for anonymous user with allow_anonymous is true  | Connection successful                            |        |          |   |
| 9            | Verify broker connection for mtconnect user with   valid password               | Connection successful                            |        |          |   |
| 10           | Verify  broker connection for mtconnect user with   invalid password             | Authentication error in broker |        |          |   |
| 11           | Verify broker connection   for hemsaw user                                  | Successful connection for hemsaw user            |        |          |   |
| 12           | Verify broker connection for invalid user                                 | Authentication error                             |        |          |   |
|