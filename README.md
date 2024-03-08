# Azure Data Lake Gen2 a simple sample with C#

Azure Data Lake Storage Gen2 is a comprehensive solution for big data analytics

It combines the capabilities of Azure Data Lake Storage Gen1 with Azure Blob Storage, enhancing performance, management, and security

The following is a simple example of how you can use C# to interact with Azure Data Lake Storage Gen2, specifically for operations like creating a file system, uploading a file, and listing the contents of a directory

## 1. Create Azure Data Lake Gen2




## 2. (Optional) Upgrade and existing Azure StorageAccount to Azure Data Lake Gen2

As previous requirement to upgrade your storageaccount, you have to disable the options soft delete blobs and container

![image](https://github.com/luiscoco/Azure_Data_Lake_Gen2/assets/32194879/b24c84f4-3c41-4287-92a8-67df93cf5dc0)


If you would like to **upgrade an existing Azure Storage Account** to become Azure Data Lake Storage Gen 2 select the following option

![image](https://github.com/luiscoco/Azure_Services_review/assets/32194879/12443c6b-9ff2-4e6a-a9eb-dd5451379632)

## 3. Sample application source code



Before you start, make sure you have the **Azure.Storage.Files.DataLake** package installed in your project

You can install it via NuGet Package Manager with the following command:

```
Install-Package Azure.Storage.Files.DataLake -Version 12.8.0
```

**Step 1: Setting up the Namespace and Authentication**

First, ensure you're using the necessary namespaces in your file:

```csharp
using Azure.Storage.Files.DataLake;
using Azure.Identity;
using System;
```

You'll also need to authenticate to your Azure Data Lake Storage Gen2 account

One common method is via Azure Active Directory (assuming you have the necessary role assignments):

```csharp
var serviceUri = new Uri("https://<your-datalake-name>.dfs.core.windows.net");
var client = new DataLakeServiceClient(serviceUri, new DefaultAzureCredential());
```

**Step 2: Creating a File System**

A file system in Azure Data Lake Storage Gen2 is equivalent to a container in Blob Storage. Here's how to create one:

```csharp
var fileSystemName = "my-file-system";
var fileSystemClient = client.GetFileSystemClient(fileSystemName);
fileSystemClient.CreateIfNotExists();
Console.WriteLine($"File system '{fileSystemName}' created or already exists.");
```

**Step 3: Uploading a File**

To upload a file, you need to get a DataLakeDirectoryClient for the directory you want to upload to (it will create the directory if it doesn't exist), and then get a DataLakeFileClient for the file itself:

```csharp
var directoryClient = fileSystemClient.GetDirectoryClient("my-directory");
directoryClient.CreateIfNotExists();

var fileClient = directoryClient.GetFileClient("sample.txt");

// Assuming you have a file named "example.txt" in your project's output directory
var filePath = "example.txt";
using var fileStream = System.IO.File.OpenRead(filePath);
fileClient.Upload(fileStream, overwrite: true);
Console.WriteLine("File uploaded.");
```

**Step 4: Listing Files in a Directory**

To list the files in a directory, you can do the following:

```csharp
var paths = directoryClient.GetPaths(recursive: false);
foreach (var path in paths)
{
    Console.WriteLine(path.Name);
}
```

This C# sample provides a basic overview of interacting with Azure Data Lake Storage Gen2

Depending on your specific requirements, you might need to explore more advanced features like setting permissions, using data lakes with data frames in analytics services, or managing file and directory properties.

