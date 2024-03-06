Salesforce SObject Types for Ballerina Salesforce Connector
===================

[![Build](https://github.com/ballerina-platform/module-ballerinax-salesforce.types/actions/workflows/ci.yml/badge.svg)](https://github.com/ballerina-platform/module-ballerinax-salesforce.types/actions/workflows/ci.yml)
[![Trivy](https://github.com/ballerina-platform/module-ballerinax-salesforce.types/actions/workflows/trivy-scan.yml/badge.svg)](https://github.com/ballerina-platform/module-ballerinax-salesforce.types/actions/workflows/trivy-scan.yml)
[![GitHub Last Commit](https://img.shields.io/github/last-commit/ballerina-platform/module-ballerinax-salesforce.types.svg)](https://github.com/ballerina-platform/module-ballerinax-salesforce.types/commits/main)
[![GraalVM Check](https://github.com/ballerina-platform/module-ballerinax-salesforce.types/actions/workflows/build-with-bal-test-graalvm.yml/badge.svg)](https://github.com/ballerina-platform/module-ballerinax-salesforce.types/actions/workflows/build-with-bal-test-graalvm.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Salesforce is a leading customer relationship management (CRM) platform that helps businesses manage and streamline their sales, service, and marketing operations. The [Ballerina Salesforce Connector](https://central.ballerina.io/ballerinax/salesforce/latest) is a project designed to enhance integration capabilities with Salesforce by providing a seamless connection for Ballerina. Notably, this Ballerina project incorporates record type definitions for the base types of Salesforce objects, offering a comprehensive and adaptable solution for developers working on Salesforce integration projects.

## Setup Guide

To customize this project for your Salesforce account and include your custom SObjects, follow the steps below:

### Step 1: Login to Your Salesforce Developer Account

Begin by logging into your [Salesforce Developer Account](https://developer.salesforce.com/).

### Step 2: Generate Open API Specification for Your SObjects

#### Step 2.1: Initiate OpenAPI Document Generation 

Use the following command to send a POST request to start the OpenAPI document generation process.

```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
https://MyDomainName.my.salesforce.com/services/data/vXX.X/async/specifications/oas3 \
-d '{"resources": ["*"]}'
```
Replace YOUR_ACCESS_TOKEN and MyDomainName with your actual access token and Salesforce domain. If successful, you'll receive a response with a URI. Extract the locator ID from the URI.

#### Step 2.2: Retrieve the OpenAPI Document

Send a GET request to fetch the generated OpenAPI document using the following command.

```bash
curl -X GET -H "Content-Type: application/json" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
https://MyDomainName.my.salesforce.com/services/data/vXX.X/async/specifications/oas3/LOCATOR_ID -o oas.json
```
Replace YOUR_ACCESS_TOKEN, MyDomainName, and LOCATOR_ID with your actual values.

### Step 3: Configure Cluster Settings

To prevent Out-of-Memory (OOM) issues, execute the following command:

```bash
export JAVA_OPTS="$JAVA_OPTS -DmaxYamlCodePoints=99999999"
```

Generate the Ballerina project for the OpenAPI spec using the Ballerina Open API tool with the following commands.

```bash
bal openapi -i oas.json --mode client --client-methods resource
```

### Step 4: Edit the Generated Client and Push it to Local Repository

#### Step 4.1 Delete the utils.bal and clients.bal files.

#### Step 4.2 Use the following commands to build, pack, and push the package:

````bash
bal pack

bal push --repository=local
````

By following these steps, you can set up and customize the Ballerina Salesforce Connector for your Salesforce account with ease.

## Quickstart

To use the `salesforce.types` module in your Ballerina application, modify the `.bal` file as follows:

### Step 1: Import the package

Import `ballerinax/salesforce.types` module.

```ballerina
import ballerinax/salesforce;
import ballerinax/salesforce.types;
```

### Step 2: Instantiate a new client

Obtain the tokens using the following the [`ballerinax/salesforce` connector set up guide](https://central.ballerina.io/ballerinax/salesforce/latest). Create a salesforce:ConnectionConfig with the obtained OAuth2 tokens and initialize the connector with it.

```ballerina
salesforce:ConnectionConfig config = {
    baseUrl: baseUrl,
    auth: {
        clientId: clientId,
        clientSecret: clientSecret,
        refreshToken: refreshToken,
        refreshUrl: refreshUrl
    }
};

salesforce:Client salesforce = new(config);
```

### Step 3: Invoke the connector operation

Now you can utilize the available operations. Note that they are in the form of remote operations. Following is an example on how to create a record using the connector.

```ballerina
salesforce:Client salesforce = check new (config);
stypes:AccountSObject response = {
   Name: "IT World",
   BillingCity: "New York"
};

salesforce:CreationResponse response = check salesforce->create("Account", response);
```

Use following command to compile and run the Ballerina program.

```bash
bal run
```

## Issues and projects

The **Issues** and **Projects** tabs are disabled for this repository as this is part of the Ballerina library. To report bugs, request new features, start new discussions, view project boards, etc., visit the Ballerina library [parent repository](https://github.com/ballerina-platform/ballerina-library).

This repository only contains the source code for the package.

## Build from the source

### Prerequisites

1. Download and install Java SE Development Kit (JDK) version 17. You can download it from either of the following sources:

    * [Oracle JDK](https://www.oracle.com/java/technologies/downloads/)
    * [OpenJDK](https://adoptium.net/)

   > **Note:** After installation, remember to set the `JAVA_HOME` environment variable to the directory where JDK was installed.

2. Download and install [Ballerina Swan Lake](https://ballerina.io/).

3. Download and install [Docker](https://www.docker.com/get-started).

   > **Note**: Ensure that the Docker daemon is running before executing any tests.

### Build options

Execute the commands below to build from the source.

1. To build the package:

   ```bash
   ./gradlew clean build
   ```
   
2. Publish the generated artifacts to the local Ballerina Central repository:

    ```bash
    ./gradlew clean build -PpublishToLocalCentral=true
    ```

3. Publish the generated artifacts to the Ballerina Central repository:

   ```bash
   ./gradlew clean build -PpublishToCentral=true
   ```

## Contribute to Ballerina

As an open-source project, Ballerina welcomes contributions from the community.

For more information, go to the [contribution guidelines](https://github.com/ballerina-platform/ballerina-lang/blob/master/CONTRIBUTING.md).

## Code of conduct

All the contributors are encouraged to read the [Ballerina Code of Conduct](https://ballerina.io/code-of-conduct).

## Useful links

* For more information go to the [`salesforce.types` package](https://lib.ballerina.io/ballerinax/salesforce.types/latest).
* For example demonstrations of the usage, go to [Ballerina By Examples](https://ballerina.io/learn/by-example/).
* Chat live with us via our [Discord server](https://discord.gg/ballerinalang).
* Post all technical questions on Stack Overflow with the [#ballerina](https://stackoverflow.com/questions/tagged/ballerina) tag.
