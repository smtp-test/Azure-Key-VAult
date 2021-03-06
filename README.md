# Azure Key Vault Extension

This connector allows the easy integration with Azure Key Vault.  All operations use the [Azure Key Vault REST API](https://docs.microsoft.com/en-us/rest/api/keyvault/) being invoked with a HTTP Client. 
## Updates
### 1.2.1 - February 23, 2022
* Fixed microsoft-trust.jks to be valid - [Issue 23](https://github.com/avioconsulting/mule-azure-key-vault-connector/issues/23)
* Added explicit connection validation before call - [Issue 27](https://github.com/avioconsulting/mule-azure-key-vault-connector/issues/27)

## Operations

### Get Secret
Retrieves the specified secret.  
Link to official documentation.  [Azure - Get Secret](https://docs.microsoft.com/en-us/rest/api/keyvault/getsecret/getsecret)

### Get Key
Retrieves the public portion of a stored key.  
Link to official documentation.  [Azure - Get Key](https://docs.microsoft.com/en-us/rest/api/keyvault/getkey/getkey) 

### Get Certificate
This operation retrieves data about a specific certificate.  
Link to official documentation.  [Azure - Get Certificate](https://docs.microsoft.com/en-us/rest/api/keyvault/getcertificate/getcertificate])

### Encrypt
This operation encrypts the value of a key provided.
Link to official documentation. [Azure - Encrypt](https://docs.microsoft.com/en-us/rest/api/keyvault/encrypt/encrypt)

### Decrypt
This operation decrypts the value of a key provided.
Link to official documentation. [Azure -Decrypt](https://docs.microsoft.com/en-us/rest/api/keyvault/decrypt/decrypt)

## Dependency
Add this dependency to your application pom.xml

```
<groupId>com.avioconsulting.mule.connector</groupId>
<artifactId>mule-azure-key-vault-connector</artifactId>
<version>1.1.0</version>
<classifier>mule-plugin</classifier>
```

## Usage
Sample calls to the key vault.  The `secretName`/`keyName`/`certificateName` attribute's are appropriate name of the secret/key/certificate.
```
<akv:get-secret config-ref="config" secretName="test-secret"/>

<akv:get-key config-ref="config" keyName="test-key"/>

<akv:get-certificate config-ref="config" certificateName="test-certificate"/>
```

### Configuration
|Parameter|Sample|Description|
|---|---|---|
|Azure OAuth Base URI|https://login.microsoftonline.com/|The base url of the login server.|
|Azure Tenant Id|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`|The Tenant ID of the Azure account.|
|Service account client id|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`|The account client id.|
|Service account client secret|`password1`|The account client secret.|
|Timeout for API calls|15000|Defaults to 30000 (30 seconds)|

To find the above configuration values, 
  * Login to the Azure Portal Account https://portal.azure.com/#home
  * Go to App Registrations under the Azure services where you can see the list of Apps registered.
  * Select the required App and click on the overview tab on the left to get the `ClientId` and `TenentId` 
  * To get the ClientSecret, navigate to home, search for key vault under Azure services, select the Key and go to secrets tab on the left
  * On the same page, go to overview tab to find the Azure key host name (Shown as DNS Name)

### To run the Test cases 

To run the test cases, use `mvn clean test -Dazure.client.id=xxxxxxxxxxxxx -Dazure.client.secret=xxxxxxxxxxxxx -Dazure.tenant.id=xxxxxxxxxxxxx -Dazure.vault.name=xxxxxxxxxxxxx`

* replace the `-Dazure.client.id`, `-Dazure.client.secret` `-Dazure.tenant.id` and `-Dazure.vault.name` with correct values before running the command.

## Deploying to Exchange
The Mule Azure Key Connector can be deployed to an Exchange with a few small modifications.
> Shamelessly stolen from Manik Mager's [blog post](https://javastreets.com/blog/publish-connectors-to-anypoint-exchange.html)
1. Update the connector pom.xml file
    * Change the `groupId` value to the Organization Id 
        * (Id found in Anypoint -> Access Management -> Organization -> You're Org)
        ```
           <modelVersion>4.0.0</modelVersion>
           <groupId>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</groupId>
           <artifactId>mule-azure-key-vault-connector</artifactId>
           <version>1.1.0</version>
           <packaging>mule-extension</packaging>
           <name>Akv Extension</name>
        ```
    * Update `distributionManagement` to point to the Exchange Repository (Uncomment these lines)
        ```
        <distributionManagement>
          <snapshotRepository>
            <id>exchange-repository</id>
            <name>Exchange Repository</name>
            <url>https://maven.anypoint.mulesoft.com/api/v1/organizations/${pom.groupId}/maven</url>
            <layout>default</layout>
          </snapshotRepository>
          <repository>
            <id>exchange-repository</id>
            <name>Exchange Repository</name>
            <url>https://maven.anypoint.mulesoft.com/api/v1/organizations/${pom.groupId}/maven</url>
            <layout>default</layout>
          </repository>
        </distributionManagement>
        ```
1. Configure your `~/.m2/settings.xml` file with your Exchange credentials
    ```
    <servers>
     <server>
       <id>exchange-repository</id>
       <username>USERNAME</username>
       <password>PASSWORD</password>
     </server>
    </servers>
    ```
1. Execute `mvn deploy` to publish to Exchange

Enjoy!