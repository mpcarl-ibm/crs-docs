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

### Getting the SDK
The easiest way to consume the AWS Java SDK is to use Maven to manage dependencies. If you aren't familiar with Maven, you get can get up and running very quickly using their [Maven in 5 Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) guide.

Maven uses a file called `pom.xml` to specify the libraries (and their versions) needed for a Java project.  Here is an example `pom.xml` file for using the AWS Java SDK to connect to IBM COS (it also includes the SoftLayer library for provisioning credentials and new accounts). 

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.cos</groupId>
    <artifactId>docs</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>docs</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-java-sdk-s3</artifactId>
            <version>1.11.5</version>
        </dependency>
        <dependency>
            <groupId>com.softlayer.api</groupId>
            <artifactId>softlayer-api-client</artifactId>
            <version>0.2.3</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.8.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>2.4.3</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```


### Creating a client and setting the endpoint

##### Create an AWS Java SDK connection using the default credentials

Use default credentials using the first credentials found in this order of precedence:

1. Environment variables
2. System properties
3. Default profile in `~/.aws/credentials`

The AWS Java SDK automatically reads the Access Key ID and Secret Access Key from one of these locations. They do not need to be provided explicitly.

{% include note.html content="The AWS Java SDK sends all requests to `s3.amazonaws.com` by default. To send requests to IBM COS, the new `AmazonS3Client` instance needs the correct `setEndpoint` parameter. The SDK interprets the http(s) portion of this endpoint and infers encrypted or plain text from the URL." %}

```java
AmazonS3 cos = new AmazonS3Client();
cos.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");
```

###### Example: Setting the endpoint to point to COS US Cross Region

```java 
AmazonS3 cos = new AmazonS3Client();
cos.setEndPoint("https://s3-api.us-geo.objectstorage.softlayer.net");
```

The COS implementation of the S3 API supports both resource path and virtual host addressing.

##### Use virtual host addressing 
This S3 implementation supports virtual host addressing of storage buckets. The AWS Java SDK uses virtual host addressing by default.

```java
AmazonS3 cos = new AmazonS3Client();
cos.setEndPoint("http://s3-api.us-geo.objectstorage.softlayer.net");
```

##### Use resource path addressing 
To configure the AWS Java SDK to use resource path addressing instead of virtual host addressing, either:

Set the `disableDNSBuckets` system property to True:

```java
System.setProperty("com.amazonaws.sdk.disableDNSBuckets", "True");
```

Set the `withPathStyleAccess` property of the `AmazonS3Client.S3ClientOptions` to `True`:
    
```java
AmazonS3 cos = new AmazonS3Client(credentials);

S3ClientOptions opts = new S3ClientOptions().withPathStyleAccess(true);
cos.setS3ClientOptions(opts);
```

#### Configure optional parameters 

##### Set the connection timeout duration 
To increase the connection timeout for longer running operations, change the `withSocketTimeout` property of the `ClientConfiguration` object when the connection is created.

```java
ClientConfiguration config = new ClientConfiguration().withSocketTimeout(15 * 60 * 1000);

AmazonS3 cos = new AmazonS3Client(credentials, config);
```

##### Set the maximum retry limit 

If AWS Java SDK receives an error from the system, it retries the request. The default number of times it retries the request is five. The default retry limit can be changed in the client application.

{% include important.html content="The client reports the final error and not the initial failure, which can mask the details of the underlying issue." %}

{% include note.html content="Set the `maxErrorRetry` property of the `ClientConfiguration` object to `0` to disable the default retry policy." %}

###### Example: Changing the Maximum Retry Value

```java
ClientConfiguration config = new ClientConfiguration().withMaxErrorRetry(0);

AmazonS3 cos = new AmazonS3Client(credentials, config);
```

### Managing credentials

The order of precedence using for access credentials is:

1. Credentials passed as `BasicAWSCredentials` instance parameters
2. Credentials set as environment variables
3. Credentials set as JVM system properties
4. Credentials set in `AwsCredentials.properties` file
5. Credentials set in the shared credentials file

##### Credentials passed as `BasicAWSCredentials` instance parameters 

Credentials can be supplied as parameters of the `BasicAWSCredentials` object. This object is then passed to the `AmazonS3Client` as a constructor parameter.

###### Example: Create an AWS Java SDK connection specifying credentials provided as strings in code

```java
final String accessKey = "lDrDjH0D45hQivu6FNlwQ"; 
final String secretKey = "bHp5DOjg0HHJrGK7h3ejEqRDnVmWZK03T4lstel6";

BasicAWSCredentials credentials = new BasicAWSCredentials(accessKey, secretKey); // declare a new set of basic credentials that includes the Access Key ID and the Secret Access Key
AmazonS3 cos = new AmazonS3Client(credentials); // create a constructor for the client using the declared credentials.
cos.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net"); // set the desired endpoint
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
AmazonS3 cos = new AmazonS3Client(); 
```

##### Credentials set as JVM system properties 

Set the JVM system properties `aws.accessKeyId` to define the access key and `aws.secretKey` to define the secret key.

These properties may be set on start up or programmatically and will be used by the `AmazonS3Client` when the default constructor is used.

###### Example: Set system properties then open an S3 connection using those properties 

```java
System.setProperty("aws.accessKeyId", "lDrDjH0D45hQivu6FNlwQ");
System.setProperty("aws.secretKey", "bHp5DOjg0HHJrGK7h3ejEqRDnVmWZK03T4lstel6");

AmazonS3 cos = new AmazonS3Client();
```

##### Credentials set in `AwsCredentials.properties` file 

An `AmazonS3Client` constructor can use a credentials file called `AwsCredentials.properties`, which is found on the Java classpath.

To create an S3 client that uses `AwsCredentials.properties`, the `AmazonS3Client` object is created by passing a `ClasspathPropertiesFileCredentialsProvider` as a constructor parameter.

###### Example: `AwsCredentials.properties` File Format

``` 
accessKey=lDrDjH0D45hQivu6FNlwQ
secretKey=bHp5DOjg0HHJrGK7h3ejEqRDnVmWZK03T4lstel6
```

###### Example: Use an `AWSCredentials.properties` file on the classpath

```java
ClasspathPropertiesFileCredentialsProvider provider = new ClasspathPropertiesFileCredentialsProvider(); // declare a new set of basic credentials that use the AWSCredentials.properties file

AmazonS3 cos = new AmazonS3Client(provider); // create a constructor for an S3 compatible client using the credentials from the AWSCredentials.properties file
cos.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net"); // set the endpoint for the new S3 compatible client
```



Credentials can be put in a different file and location. The credentials can be accessed passing the `PropertiesFileCredentialsProvider` constructor parameter when creating an `AmazonS3Client` instance.

###### Example: Creating a client using a `AwsCredentials.properties` in another location

```java 
AWSCredentialsProvider provider = new PropertiesFileCredentialsProvider("/path/to/alternative/credentials/file.properties");

AmazonS3 cos = new AmazonS3Client(provider);
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

Use `aws configure — profile {profileName}` to create an access credentials file with a named profile. Additional named profiles are appended to the `~/.aws/credentials` file.

###### Example: Command output of `aws configure --profile pool2`

``` 
$ aws configure --profile pool2
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]:
Default output format [None]: text
```

###### Example: Contents of the AWS credentials file (`~/.aws/credentials`)

```
[default]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[profile pool2]
aws_access_key_id = N67W90RKLCWOLPSKN8W8
aws_secret_access_key = RlfTDyqPg0WnY/PWdxMEe/gjuG7QRckynofRMwwR

```

###### Example: Contents of the AWS configuration file (`~/.aws/config`)

``` 
[default]
output=json

[profile pool2]
output=text
```
##### Use a named profile in a AWS shared credentials file

To use the name profile credentials in this file:

1. Provide the profile name as a parameter of the `ProfileCredentialsProvider` Object.
2. Provide the resulting object to the `AmazonS3Client` as a constructor parameter.

###### Example: Using an AWS Credential Named Profile in an AWS Java SDK connection method

```java
AWSCredentialsProvider provider = new ProfileCredentialsProvider("ibm"); // specify the Named Profile to use
AmazonS3 cos = new AmazonS3Client(provider); // specify which set of credentials to use
```

### Code examples

{% include warning.html content="These are examples. They should be used to assist developers in programming their own solutions and not be copied and pasted directly into their applications. IBM cannot be held accountable for developers using this code verbatim." %}

Note: The remaining examples assume the use of default credentials.


#### Create a bucket

``` 
Example: Create a Bucket in a system using the s3Client.createBucket() method

AmazonS3 s3Client = new AmazonS3Client();
s3Client.setEndpoint("https://s3-api.us-geo.objectstorage.softlayer.net");

s3Client.createBucket("sample", "us-standard"); 1

1 The name of the Bucket to be created.
```

#### Upload object from a file

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

#### Upload object using a stream

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

#### Download object to a file

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


#### Download object using a stream

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

#### Delete object

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

#### Copy objects


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

#### List objects 

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

#### List buckets 

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
