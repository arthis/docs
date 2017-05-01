---
title: "Generating a WCF Client from Service Metadata | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 27f8f545-cc44-412a-b104-617e0781b803
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
---
# Generating a WCF Client from Service Metadata
This topic describes how to use the various switches in Svcutil.exe to generate clients from metadata documents.  
  
 Metadata documents can be on a durable storage or be retrieved online. Online retrieval follows either the WS-MetadataExchange protocol or the Microsoft Discovery (DISCO) protocol. Svcutil.exe issues the following metadata requests simultaneously to retrieve metadata:  
  
-   WS-MetadataExchange (MEX) request to the supplied address.  
  
-   MEX request to the supplied address with `/mex` appended.  
  
-   DISCO request (using the [DiscoveryClientProtocol](http://go.microsoft.com/fwlink/?LinkId=94777) from ASP.NET Web services) to the supplied address.  
  
 Svcutil.exe generates the client based on the Web Services Description Language (WSDL) or policy file received from the service. The user principal name (UPN) is generated by concatenating the user name with "@" and then adding a fully-qualified domain name (FQDN). However, for users who registered on Active Directory, this format is not valid and the UPN that the tool generates causes a failure in the Kerberos authentication with the following error message: **The logon attempt failed.** To resolve this problem, manually fix the client file that the tool generated.  
  
```  
svcutil.exe [/t:code]  <metadataDocumentPath>* | <url>* | <epr>  
```  
  
## Referencing and Sharing Types  
  
|Option|Description|  
|------------|-----------------|  
|**/reference:\<file path>**|References types in the specified assembly. When generating clients, use this option to specify assemblies that might contain types that represent the metadata being imported.<br /><br /> Short form: `/r`|  
|**/excludeType:\<type>**|Specifies a fully-qualified or assembly-qualified type name to be excluded from referenced contract types.<br /><br /> Short form: `/et`|  
  
## Choosing a Serializer  
  
|Option|Description|  
|------------|-----------------|  
|**/serializer:Auto**|Automatically selects the serializer. This uses the `DataContract` serializer. If this fails, the `XmlSerializer` is used.<br /><br /> Short Form: `/ser:Auto`|  
|**/serializer:DataContractSerializer**|Generates data types that use the `DataContract` serializer for serialization and deserialization.<br /><br /> Short form: `/ser:DataContractSerializer`|  
|**/serializer:XmlSerializer**|Generates data types that use the `XmlSerializer` for serialization and deserialization.<br /><br /> Short form: `/ser:XmlSerializer`|  
|**/importXmlTypes**|Configures the `DataContract` serializer to import non-`DataContract` types as `IXmlSerializable` types.<br /><br /> Short form: `/ixt`|  
|**/dataContractOnly**|Generates code for `DataContract` types only. `ServiceContract` types are generated.<br /><br /> You should specify only local metadata files for this option.<br /><br /> Short form: `/dconly`|  
  
## Choosing a Language for the Client  
  
|Option|Description|  
|------------|-----------------|  
|**/language:\<language>**|Specifies the programming language to use for code generation. Provide either a language name registered in the Machine.config file or the fully-qualified name of a class that inherits from <xref:System.CodeDom.Compiler.CodeDomProvider>.<br /><br /> Values: c#, cs, csharp, vb, vbs, visualbasic, vbscript, javascript, c++, mc, cpp<br /><br /> Default: csharp<br /><br /> Short form: `/l`<br /><br /> [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [CodeDomProvider Class](http://go.microsoft.com/fwlink/?LinkId=94778).|  
  
## Choosing a Namespace for the Client  
  
|Option|Description|  
|------------|-----------------|  
|**/namespace:\<string,string>**|Specifies a mapping from a WSDL or XML Schema `targetNamespace` to a common language runtime (CLR) namespace. Using a wildcard (*) for the `targetNamespace` maps all `targetNamespaces` without an explicit mapping to that CLR namespace.<br /><br /> To make sure that the message contract name does not collide with the operation name, either qualify the type reference with double colons (`::`) or make sure the names are unique.<br /><br /> Default: Derived from the target namespace of the schema document for `DataContracts`. The default namespace is used for all other generated types.<br /><br /> Short form: `/n`|  
  
## Choosing a Data Binding  
  
|Option|Description|  
|------------|-----------------|  
|**/enableDataBinding**|Implements the <xref:System.ComponentModel.INotifyPropertyChanged> interface on all `DataContract` types to enable data binding.<br /><br /> Short form: `/edb`|  
  
## Generating Configuration  
  
|Option|Description|  
|------------|-----------------|  
|**/config:\<configFile>**|Specifies the file name for the generated configuration file.<br /><br /> Default: output.config|  
|**/mergeConfig**|Merges the generated configuration into an existing file, instead of overwriting the existing file.|  
|**/noConfig**|Do not generate configuration files.|  
  
## See Also  
 [Using Metadata](../../../../docs/framework/wcf/feature-details/using-metadata.md)   
 [Metadata Architecture Overview](../../../../docs/framework/wcf/feature-details/metadata-architecture-overview.md)