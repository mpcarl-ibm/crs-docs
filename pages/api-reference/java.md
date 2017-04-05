---
title: Using Java with IBM COS
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: java
redirect_from:
  - /crs-java
  - /crs-java.html
folder: api-reference
toc: true
---
foo

### Getting the SDK
foo

### Managing credentials

The order of precedence using for access credentials is:

1. Credentials passed as `BasicAWSCredentials` instance parameters
2. Credentials set as environment variables
3. Credentials set as JVM system properties
4. Credentials set in `AwsCredentials.properties` file
5. Credentials set in the shared credentials file

##### Credentials passed as `BasicAWSCredentials` instance parameters 

Credentials can be supplied as parameters of the `BasicAWSCredentials` object. This object is then passed to the `AmazonS3Client` as a constructor parameter.

Example: Setting and loading credentials using `BasicAWSCredentials` instance parameters

```java
final String accessKey = "lDrDjH0D45hQivu6FNlwQ";
final String secretKey = "bHp5DOjg0HHJrGK7h3ejEqRDnVmWZK03T4lstel6";

BasicAWSCredentials credentials = new BasicAWSCredentials(accessKey, secretKey);
AmazonS3Client cosClient = new AmazonS3Client(credentials);
```

##### Credentials set as environment variables 

Set the environment variables `AWS_ACCESS_KEY_ID` to define the access key and `AWS_SECRET_ACCESS_KEY` to define the secret key.

If these variables are defined when the `AmazonS3Client` is created, the default constructor can be used.

{% include note.html content="Setting environment variables varies by operating system. Refer to this [Knowledge Base Article](http://www.schrodinger.com/kb/1842 ) for more information." %}




| Variable     |            Purpose
|-------------------------------------
| `AWS_ACCESS_KEY`      |      AWS access key.
| `AWS_SECRET_ACCESS_KEY`  |   AWS secret key. Access and secret key variables override credentials that are stored in both credential and config files.
| `AWS_PROFILE`          |    Name of the profile to use, such as the name of a profile that is stored in a credential or a config file. The default is to use the default profile.
{:.opstable}


###### Example: Setting and loading credentials using system Environment Variables

The AWS Java SDK automatically reads the Access Key ID and Secret Access Key from the Environment Variables. They do not need to be provided explicitly.

```java
AmazonS3 s3Client = new AmazonS3Client(); 
```

##### Credentials set as JVM system properties 

Set the JVM system properties `aws.accessKeyId` to define the access key and `aws.secretKey` to define the secret key.

These properties may be set on start up or programmatically and will be used by the `AmazonS3Client` when the default constructor is used.

###### Example: Set system properties then open an S3 connection using those properties 

```java
System.setProperty("aws.accessKeyId", "lDrDjH0D45hQivu6FNlwQ");
System.setProperty("aws.secretKey", "bHp5DOjg0HHJrGK7h3ejEqRDnVmWZK03T4lstel6");

AmazonS3 cosClient = new AmazonS3Client();
```


##### Credentials set in `AwsCredentials.properties` file 

An `AmazonS3Client` constructor can use a credentials file called `AwsCredentials.properties`, which is found on the Java classpath.

To create an S3 client that uses `AwsCredentials.properties`, the `AmazonS3Client` object is created by passing a `ClasspathPropertiesFileCredentialsProvider` as a constructor parameter.

###### Example: `AwsCredentials.properties` File Format

``` 
accessKey=lDrDjH0D45hQivu6FNlwQ
secretKey=bHp5DOjg0HHJrGK7h3ejEqRDnVmWZK03T4lstel6
```

###### Example: Creating a client using `AwsCredentials.properties` in the Java Classpath

```java
AWSCredentialsProvider provider = new ClasspathPropertiesFileCredentialsProvider();

AmazonS3 cosClient = new AmazonS3Client(provider);
```


Credentials can be put in a different file and location. The credentials can be accessed passing the `PropertiesFileCredentialsProvider` constructor parameter when creating an `AmazonS3Client` instance.

###### Example: Creating a client using a `AwsCredentials.properties` in another location

``` 
AWSCredentialsProvider provider = new PropertiesFileCredentialsProvider(
"/path/to/alternative/credentials/file.properties");

AmazonS3 cosClient = new AmazonS3Client(provider);
```


##### Credentials set in the shared credentials file 

###### Create AWS shared credentials 

Use `aws configure` to create an access credentials file with the default profile. For more information about using the AWS CLI, see the documentation on [using command line interfaces].

###### Example: Command output of `aws configure`
``` 
$ aws configure
AWS Access Key ID [None]: #AKIAIOSFODNN7EXAMPLE# // stored in ~/.aws/credentials
AWS Secret Access Key [None]: #wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY# // stored in ~/.aws/credentials
Default region name [None]: // the region does not need to be set
Default output format [None]: #json# // the output format will be stored in ~/.aws/config
```




To support multiple identities, AWS credential files have named profiles.

Use aws configure — profile {profileName} to create an access credentials file with a named profile. Additional named profiles are appended to the \~/.aws/credentials file.



``` 
Example: Command output of aws configure --profile pool2

$ aws configure --profile pool2
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE 1
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY 2
Default region name [None]: 3
Default output format [None]: json 4

1 The Access Key will be stored in ~/.aws/credentials.
2 The Secret Key will be stored in ~/.aws/credentials.
3 The Region does not need to be set. The user can press the Return key 
  to skip this. The Region will be stored in ~/.aws/config.
4 The Output Format will be stored in ~/.aws/config.
```





``` 
Example: Contents of the AWS credentials file (~/.aws/credentials)

[default]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[profile pool2] 1
aws_access_key_id = N67W90RKLCWOLPSKN8W8
aws_secret_access_key = RlfTDyqPg0WnY/PWdxMEe/gjuG7QRckynofRMwwR

1 The pool2 profile offers another set of Access Credentials for an S3 connection.
```





``` 
Example: Contents of the AWS configuration file (~/.aws/config)

[default]
output=json

[profile pool2]
output=text
```








#### Use a named profile in a AWS shared credentials file





To use the name profile credentials in this file:

1. Provide the profile name as a parameter of the `ProfileCredentialsProvider` Object.
2. Provide the resulting object to the `AmazonS3Client` as a constructor parameter.

###### Example: Using an AWS Credential Named Profile in an AWS Java SDK connection method

``` 
AWSCredentialsProvider provider = new ProfileCredentialsProvider("ibm"); // specify the Named Profile to use
AmazonS3 s3Client = new AmazonS3Client(provider); // pecify which set of credentials to use
```



### Creating a client and setting the endpoint
foo

### Example code
foo

### API reference

This list summarizes the AWS Java SDK methods that are supported with a system. More detailed documentation on individual classes and methods can be found in the [GitHub repository](https://github.com/aws/aws-sdk-java).

```java
abortMultipartUpload(AbortMultipartUploadRequest request)
completeMultipartUpload(CompleteMultipartUploadRequest request)
copyObject(CopyObjectRequest copyObjectRequest)
copyObject(String sourceBucketName, String sourceKey, String destinationBucketName, String destinationKey)
copyPart(CopyPartRequest copyPartRequest)
createBucket(CreateBucketRequest createBucketRequest)
createBucket(String bucketName)
createBucket(String bucketName, String LocationConstraint)
deleteBucket(DeleteBucketRequest deleteBucketRequest)
deleteBucket(String bucketName)
deleteBucketCrossOriginConfiguration(DeleteBucketCrossOriginConfigurationRequest deleteBucketCrossOriginConfigurationRequest)
deleteBucketCrossOriginConfiguration(String bucketName)
deleteObject(DeleteObjectRequest deleteObjectRequest)
deleteObject(String bucketName, String key)
deleteObjects(DeleteObjectsRequest deleteObjectsRequest)
doesBucketExist(String bucketName)
generatePresignedUrl(GeneratePresignedUrlRequest generatePresignedUrlRequest)
generatePresignedUrl(String bucketName, String key, Date expiration)
generatePresignedUrl(String bucketName, String key, Date expiration, HttpMethod method)
getBucketAcl(GetBucketAclRequest getBucketAclRequest)
getBucketAcl(String bucketName)
getBucketCrossOriginConfiguration(String bucketName)
getBucketLocation(GetBucketLocationRequest getBucketLocationRequest)
getBucketLocation(String bucketName)
getCachedResponseMetadata(AmazonWebServiceRequest request)
getObject(GetObjectRequest getObjectRequest)
getObject(GetObjectRequest getObjectRequest, File destinationFile)
getObject(String bucketName, String key)
getObjectAcl(String bucketName, String key)
getObjectAcl(String bucketName, String key, String versionId)
getObjectMetadata(GetObjectMetadataRequest getObjectMetadataRequest)
getObjectMetadata(String bucketName, String key)
initiateMultipartUpload(InitiateMultipartUploadRequest request)
listBuckets()
listBuckets(ListBucketsRequest listBucketsRequest)
listMultipartUploads(ListMultipartUploadsRequest request)
listNextBatchOfObjects(ObjectListing previousObjectListing)
listNextBatchOfVersions(VersionListing previousVersionListing)
listObjects(String bucketName)
listObjects(String bucketName, String prefix)
listObjects(ListObjectsRequest listObjectsRequest)
listParts(ListPartsRequest request)
putObject(String bucketName, String key, File file)
putObject(String bucketName, String key, InputStream input, ObjectMetadata metadata)
putObject(PutObjectRequest putObjectRequest)
setBucketAcl(String bucketName, AccessControlList acl)
setBucketAcl(String bucketName, CannedAccessControlList acl)
setBucketAcl(SetBucketAclRequest setBucketAclRequest)
setBucketCrossOriginConfiguration(String bucketName, BucketCrossOriginConfiguration bucketCrossOriginConfiguration)
setBucketCrossOriginConfiguration(SetBucketCrossOriginConfigurationRequest setBucketCrossOriginConfigurationRequest)
setEndpoint(String endpoint)
setObjectAcl(String bucketName, String key, AccessControlList acl)
setObjectAcl(String bucketName, String key, CannedAccessControlList acl)
setObjectAcl(String bucketName, String key, String versionId, AccessControlList acl)
setObjectAcl(String bucketName, String key, String versionId, CannedAccessControlList acl)
setObjectAcl(SetObjectAclRequest setObjectAclRequest)
setS3ClientOptions(S3ClientOptions clientOptions)
uploadPart(UploadPartRequest request)
```


### Configure the storage endpoint and host addressing scheme 


The AWS Java SDK sends all requests to s3.amazonaws.com by default.

To send requests to a system, change the AmazonS3Client to use a different address.

To send requests to a system, create a new AmazonS3Client instance that uses a different setEndpoint parameter with a different hostname or IP address.



Note: The AWS Java SDK interprets the http(s) portion of this endpoint and infers encrypted or plain text from the URL.



The CSO API supports both resource path and virtual host addressing.



``` 
Example: Setting a system as an S3 endpoint in .Java

The setEndpoint method can be used to point to a Accesser node that is assigned IP address
192.168.9.230:

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndPoint("http://192.168.9.230");
```






#### Use Virtual Host Addressing 



This S3 implementation supports virtual host addressing of storage buckets. The AWS Java SDK uses virtual host addressing by default.

The CSO API supports virtual host addressing of buckets. To provide access, use a domain name and not an IP address for the Accesser node.

The bucket name is determined by the virtual subdomain that is used to connect to the service.



``` 
Example: Using a Domain Name for Virtual Host Addressing

The Virtual Subdomain is determined by the bucket name.

A User can use http://bucketname.somedomain.com/ to access the 'bucket' named 'bucketname'. 
'bucketname' is the S3 Virtual Host Suffix.
```





To use virtual-host addressing with a system:
1.  Configure their DNS servers properly to perform proper routing of virtual host style addresses to the appropriate Accesser node or load balancer IP address.
2.  Configure an <span class="ph uicontrol">S3 Virtual Host Suffix in the Manager Web Interface for each access pool that is used to service virtual host addressed requests.
    

    Note: See Create Access Pool in the Manager Administration Guide for more details on setting this parameter in a storage pool.

    

3.  Set a domain name and not an IP address for the endpoint.

``` 
AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndPoint("http://s3-api.us-geo.objectstorage.softlayer.net");
```








#### Use resource path addressing {#use-resource-path-addressing 



To configure the AWS Java SDK to use resource path addressing instead of virtual host addressing, either:

Set the disableDNSBuckets system property to True:
    

    ``` 
    Example: Setting the disableDNSBuckets System Property Separately

    System.setProperty("com.amazonaws.sdk.disableDNSBuckets", "True");
    ```

    

Set the withPathStyleAccess property of the AmazonS3Client.S3ClientOptions to True:
    

    ``` 
    Example: Setting the withPathStyleAccess Property when Opening the Connection

    AmazonS3 s3 = new AmazonS3Client(credentials);

    S3ClientOptions opts = new S3ClientOptions().withPathStyleAccess(true);
    s3.setS3ClientOptions(opts);
    ```

    








### Configure optional parameters 





#### Set the connection timeout duration 


To increase the connection timeout for longer running operations, change the withSocketTimeout property of the ClientConfiguration object when the connection is created.



``` 
ClientConfiguration config = new ClientConfiguration().withSocketTimeout(15 * 60 * 1000);

AmazonS3 s3 = new AmazonS3Client(credentials, config);
```








#### Set the Maximum Retry Limit 



If AWS Java SDK receives an error from the system, it retries the request. The default number of times it retries the request is five. The default retry limit can be changed in the client application.


CAUTION:

The client reports the final error and not the initial failure, which can mask the details of the underlying issue.





Note: Set the maxErrorRetry property of the ClientConfiguration object to 0 to disable the default retry policy.





``` 
Example: Changing the Maximum Retry Value

ClientConfiguration config = new ClientConfiguration().withMaxErrorRetry(0);

AmazonS3 s3 = new AmazonS3Client(credentials, config);
```






# Java Methods 

## Supported methods 



### Examples 

{% include warning.html content="These are examples. They should be used to assist developers in programming their own solutions and not be copied and pasted directly into their applications. IBM cannot be held accountable for developers using this code verbatim." %}

#### Create an AWS Java SDK connection

##### Create an AWS Java SDK Connection Using the Default Credentials

Use default credentials using the first credentials found in this order of precedence:

1. Environment Variables
2. System Properties
3. Default Profile in ~/.aws/credentials

The AWS Java SDK automatically reads the Access Key ID and Secret Access Key from one of these locations. They do not need to be provided explicitly.

``` 
AmazonS3 cosClient = new AmazonS3Client();
cosClient.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");
```

```
Example: Create an AWS Java SDK Connection Specifying Credentials 
         Provided as Strings in Code

BasicAWSCredentials credentials = new BasicAWSCredentials( 1
"ACCESS_KEY", 2
"SECRET_KEY" 3
);
AmazonS3 s3Client = new AmazonS3Client(credentials); 4
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net"); 5

1 Declare a new set of basic credentials that…
2 Include the AWS Access Key ID.
3 Include the Secret Access Key.
4 Create a constructor for an S3-compatible client using the declared credentials.
5 Set the endpoint for the new S3 compatible system to 'https://s3-api.us-geo.objectstorage.softlayer.net'.
```

```
Example: Use an AWSCredentials.properties file on the classpath

Credentials are loaded from the AWSCredentials.properties File

ClasspathPropertiesFileCredentialsProvider provider = new
ClasspathPropertiesFileCredentialsProvider(); 1

AmazonS3 s3Client = new AmazonS3Client(provider); 2
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net"); 3

1 Declare a new set of basic credentials that use the AWSCredentials.properties file.
2 Create a constructor for an S3 compatible client using the credentials from the
  AWSCredentials.properties file.
3 Set the endpoint for the new S3 compatible client to 'https://s3-api.us-geo.objectstorage.softlayer.net'.
```

Note: The remaining examples assume the use of default credentials.


## Create a bucket

``` 
Example: Create a Bucket in a system using the s3Client.createBucket() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");

s3Client.createBucket("sample"); 1

1 The name of the Bucket to be created.
```

## Upload object from a file

Note: This example assumes that the Bucket sample already exists.

```
Example: Upload an Object from a File to a Bucket in a system using the s3Client.putObject() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");

s3Client.putObject(
"sample", 1
"myfile", 2
new File("/home/user/test.txt") 3
);

1 The name of the Bucket into which the Object will be placed.
2 The name of the Object once it has been uploaded.
3 The file name and path of the file to be uploaded.
```

## Upload object using a stream

Note: This example assumes that the Bucket sample already exists.

```
Example: Upload an Object from an I/O Stream to a Bucket in a system using the s3Client.putObject() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");
String obj = "An example"; 1
ByteArrayOutputStream theBytes = new ByteArrayOutputStream(); 2
ObjectOutputStream serializer = new ObjectOutputStream(theBytes); 3
serializer.writeObject(obj); 4
serializer.flush();
serializer.close();
InputStream stream = new ByteArrayInputStream(theBytes.toByteArray()); 5
ObjectMetadata metadata = new ObjectMetadata(); 6
metadata.setContentType("application/x-java-serialized-object"); 7
metadata.setContentLength(theBytes.size()); 8
s3Client.putObject(
"sample", 9
"serialized-object", 10
stream, 11
metadata 12
);

1 An object to be stored.
2 Create a new output stream to store the object data.
3 Set the object data to be serialized.
4 Serialize the object data.
5 Convert the serialized object data into a new input stream to store.
6 Define the metadata about this object to be stored with it.
7 Set the metadata for the object being written.
8 Set metadata for the length of the data stream.
9 The name of the Bucket to which the Object is being written.
10 The name of the Object being written.
11 The name of the data stream writing the Object.
12 The metadata for the Object being written.
```

## Download object to a file {#download-object-to-a-file .title .topictitle2}

Note: This example assumes the Bucket sample already exists.


```
Example: Download an Object from a Bucket to a File using the s3Client.getObject() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");

GetObjectRequest request = new 1
GetObjectRequest( 2
"sample", 3
"myFile" 4
);

s3Client.getObject( 5
request, 6
new File("retrieved.txt") 7
);

1 Create a new request to get an object.
2 Request the object by identifying…
3 The sample Bucket
4 The myFile Object
5 Write the contents of the Object…
6 Using the request that was just created
7 To write to a new file called retrieved.txt
```


## Download object using a stream {#download-object-using-a-stream .title .topictitle2}

Note: This example assumes that the Bucket sample already exists.

``` 
Example: Download an object using a stream

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");
S3Object returned = CLIENT.getObject( 1
"sample", 2
"serialized-object" 3
);
S3ObjectInputStream s3Input = s3Response.getObjectContent(); 4

1 Request the object by identifying…
2 The sample Bucket
3 The serialized-object Object
4 Set the object stream.
```

## Delete object {#delete-object .title .topictitle2}

``` 
Example: Delete an Object from a Bucket using the s3Client.deleteObject() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");

s3Client.deleteObject( 1
"sample", 2
"myFile.txt" 3
);

1 Delete the Object, passing…
2 The name of the Bucket that stores the Object.
3 The name of the Object to be deleted.
```

## Copy objects {#copy-objects .title .topictitle2}


```
Example: Copy an Object within the Same Bucket using the s3Client.copyObject() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");

// Copy an object within the same Bucket
s3Client.copyObject( 1
"sample", 2
"myFile.txt", 3
"sample", 4
"myFile.txt.backup" 5
);

1 Copy the Object, passing…
2 The name of the Bucket in which the Object to be copied is stored.
3 The name of the Object being copied from the source Bucket.
4 The name of the Bucket in which the Object to be copied is stored.
5 The new name of the copy of the Object to be copied.
```

```
Example: Copy an Object between Two Buckets using the s3Client.copyObject() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");

// Copy an object between two Buckets
s3Client.copyObject( 1
"sample", 2
"myFile.txt", 3
"backup", 4
"myFile.txt" 5
);

1 Copy the Object, passing…
2 The name of the Bucket from which the Object will be copied.
3 The name of the Object being copied from the source Bucket.
4 The name of the Bucket to which the Object will be copied.
5 The name of the copied Object in the destination Bucket.
```

## List objects 

``` 
Example: Listing Buckets using the s3Client.listObjects() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint(`https://s3-api.us-geo.objectstorage.softlayer.net`);

ObjectListing listing = s3Client.listObjects(`sample`); 1
List<S3ObjectSummary> summaries = listing.getObjectSummaries(); 2

for (S3ObjectSummary obj : summaries){ 3
  System.out.println(`found: `+obj.getKey()); 4
}

1 Get the list of Objects in the sample Bucket.
2 Create a list of Object Summaries.
3 For each Object…
4 Display `found: ` then the name of the Object.
```

## List buckets 

```
Example: Listing Buckets using the s3Client.listBuckets() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint(`https://s3-api.us-geo.objectstorage.softlayer.net`);

List<Bucket> Buckets = s3Client.listBuckets(); 1

for (Bucket b : Buckets) { 2
  System.out.println(`Found: `+b.getName()); 3
}

1 Get a list of Buckets.
2 For each bucket…
3 Display `Found: ` then the name of the Bucket.
```
