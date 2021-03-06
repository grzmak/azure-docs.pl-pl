---
title: Azure Service Fabric usługi modelu XML schematu atrybutu grup | Dokumentacja firmy Microsoft
description: Zawiera opis grupy atrybutów w schemacie XML modelu usługi sieć szkieletowa usług.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/06/2018
ms.author: ryanwi
ms.openlocfilehash: 012bcd42772b28ab14b26d0c8f5c4396e66a43b7
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809849"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-attribute-groups"></a>Grupy atrybutów schematu XML modelu usług

## <a name="accountcredentialsgroup-attributegroup"></a>AccountCredentialsGroup attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (2)|
|name|AccountCredentialsGroup|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="AccountCredentialsGroup">
        <xs:attribute name="AccountName" type="xs:string" use="optional">
            <xs:annotation>
                <xs:documentation>User name or Service Account Name (for example, MyMachine\JohnDoe or John.Doe@department.contoso.com).</xs:documentation>
            </xs:annotation>
        </xs:attribute>
        <xs:attribute name="Password" type="xs:string" use="optional">
            <xs:annotation>
                <xs:documentation>Password for the user account.</xs:documentation>
            </xs:annotation>
        </xs:attribute>
    </xs:attributeGroup>
    
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="accountname"></a>Nazwa konta
Nazwa użytkownika lub nazwa konta usługi (np. Mój_komputer\jankowalski lub John.Doe@department.contoso.com).
|Atrybut|Wartość|
|---|---|
|name|Nazwa konta|
|type|xs:String|
|Użyj|opcjonalne|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="AccountName" type="xs:string" use="optional">
            <xs:annotation>
                <xs:documentation>User name or Service Account Name (for example, MyMachine\JohnDoe or John.Doe@department.contoso.com).</xs:documentation>
            </xs:annotation>
        </xs:attribute>
        
```

#### <a name="password"></a>Hasło
Hasło dla konta użytkownika.
|Atrybut|Wartość|
|---|---|
|name|Hasło|
|type|xs:String|
|Użyj|opcjonalne|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Password" type="xs:string" use="optional">
            <xs:annotation>
                <xs:documentation>Password for the user account.</xs:documentation>
            </xs:annotation>
        </xs:attribute>
    
```

## <a name="applicationinstanceattrgroup-attributegroup"></a>ApplicationInstanceAttrGroup attributeGroup
Grupa atrybutów wystąpienia aplikacji.

|Atrybut|Wartość|
|---|---|
|content|atrybuty (2)|
|name|ApplicationInstanceAttrGroup|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationInstanceAttrGroup">
    <xs:annotation>
      <xs:documentation>Attribute group for application instance.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="NameUri" type="FabricUri" use="required">
      <xs:annotation>
        <xs:documentation>Fully qualified name of the application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    <xs:attribute name="ApplicationId" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>ID of this application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="nameuri"></a>NameUri
Pełna nazwa aplikacji.
|Atrybut|Wartość|
|---|---|
|name|NameUri|
|type|FabricUri|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="NameUri" type="FabricUri" use="required">
      <xs:annotation>
        <xs:documentation>Fully qualified name of the application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    
```

#### <a name="applicationid"></a>ApplicationId
Identyfikator tej aplikacji.
|Atrybut|Wartość|
|---|---|
|name|ApplicationId|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationId" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>ID of this application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="applicationmanifestattrgroup-attributegroup"></a>ApplicationManifestAttrGroup attributeGroup
Grupa atrybutów manifestu aplikacji.

|Atrybut|Wartość|
|---|---|
|content|atrybuty (3)|
|name|ApplicationManifestAttrGroup|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationManifestAttrGroup">
    <xs:annotation>
      <xs:documentation>Attribute group for application manifest.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="ApplicationTypeName" use="required">
      <xs:annotation>
        <xs:documentation>The type identifier for this application.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="ApplicationTypeVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of this application type, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="ManifestId" use="optional" default="" type="xs:string">
      <xs:annotation>
        <xs:documentation>The identifier of this application manifest, an unstructured string.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    <xs:anyAttribute processContents="skip"/> <!-- Allow unknown attributes to be used. -->
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="applicationtypename"></a>ApplicationTypeName
Identyfikator typu dla tej aplikacji.
|Atrybut|Wartość|
|---|---|
|name|ApplicationTypeName|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationTypeName" use="required">
      <xs:annotation>
        <xs:documentation>The type identifier for this application.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="applicationtypeversion"></a>ApplicationTypeVersion
Wersja tego typu aplikacji ciągiem bez struktury.
|Atrybut|Wartość|
|---|---|
|name|ApplicationTypeVersion|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationTypeVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of this application type, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="manifestid"></a>ManifestId
Identyfikator tego manifestu aplikacji ciągiem bez struktury.
|Atrybut|Wartość|
|---|---|
|name|ManifestId|
|Użyj|opcjonalne|
|default||
|type|xs:String|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ManifestId" use="optional" default="" type="xs:string">
      <xs:annotation>
        <xs:documentation>The identifier of this application manifest, an unstructured string.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    
```

## <a name="configoverridesidentifier-attributegroup"></a>ConfigOverridesIdentifier attributeGroup
Identyfikuje operacje zastępowania konfiguracji dla pakietu usług.

|Atrybut|Wartość|
|---|---|
|content|atrybuty (2)|
|name|ConfigOverridesIdentifier|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ConfigOverridesIdentifier">
    <xs:annotation>
      <xs:documentation>Identifies configuration overrides for a service package.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="ServicePackageName" type="xs:string" use="required"/>
    <xs:attribute name="RolloutVersion" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>ID of the rollout in which changes were made to the overrides element.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="servicepackagename"></a>ServicePackageName
|Atrybut|Wartość|
|---|---|
|name|ServicePackageName|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServicePackageName" type="xs:string" use="required"/>
    
```

#### <a name="rolloutversion"></a>RolloutVersion
Identyfikator wdrożenia, w którym wprowadzono zmiany elementu operacji zastępowania.
|Atrybut|Wartość|
|---|---|
|name|RolloutVersion|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RolloutVersion" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>ID of the rollout in which changes were made to the overrides element.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="connectionstring-attributegroup"></a>ConnectionString attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|Parametry połączenia|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ConnectionString">
                <xs:attribute name="ConnectionString" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Connection string to the Azure storage account. Format:
          DefaultEndpointsProtocol=https;AccountName=[];AccountKey=[]</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="connectionstring"></a>Parametry połączenia
Parametry połączenia z kontem magazynu platformy Azure. Format: DefaultEndpointsProtocol = https; AccountName =] AccountKey =]
|Atrybut|Wartość|
|---|---|
|name|Parametry połączenia|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ConnectionString" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Connection string to the Azure storage account. Format:
          DefaultEndpointsProtocol=https;AccountName=[];AccountKey=[]</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="containername-attributegroup"></a>Właściwość ContainerName attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|ContainerName|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ContainerName">
    <xs:attribute name="ContainerName" type="xs:string">
      <xs:annotation>
        <xs:documentation>The name of the container in Azure blob storage where data is uploaded.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="containername"></a>ContainerName
Nazwa kontenera w magazynie obiektów blob platformy Azure, którego dane są przesyłane.
|Atrybut|Wartość|
|---|---|
|name|ContainerName|
|type|xs:String|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ContainerName" type="xs:string">
      <xs:annotation>
        <xs:documentation>The name of the container in Azure blob storage where data is uploaded.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="datadeletionageindays-attributegroup"></a>DataDeletionAgeInDays attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|DataDeletionAgeInDays|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="DataDeletionAgeInDays">
    <xs:attribute name="DataDeletionAgeInDays" type="xs:string">
      <xs:annotation>
        <xs:documentation>Number of days after which old data is deleted from this location.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="datadeletionageindays"></a>DataDeletionAgeInDays
Liczba dni, po upływie których stare dane są usuwane z tej lokalizacji.
|Atrybut|Wartość|
|---|---|
|name|DataDeletionAgeInDays|
|type|xs:String|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="DataDeletionAgeInDays" type="xs:string">
      <xs:annotation>
        <xs:documentation>Number of days after which old data is deleted from this location.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="isenabled-attributegroup"></a>IsEnabled attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|IsEnabled|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="IsEnabled">
                <xs:attribute name="IsEnabled" type="xs:string">
                        <xs:annotation>
                                <xs:documentation>Whether or not data transfer to this destination is enabled. By default, it is not enabled.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        </xs:attributeGroup>
        
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="isenabled"></a>IsEnabled
Określa, czy włączono transfer danych do tego miejsca docelowego. Domyślnie ta jest wyłączona.
|Atrybut|Wartość|
|---|---|
|name|IsEnabled|
|type|xs:String|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="IsEnabled" type="xs:string">
                        <xs:annotation>
                                <xs:documentation>Whether or not data transfer to this destination is enabled. By default, it is not enabled.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        
```

## <a name="levelfilter-attributegroup"></a>LevelFilter attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|LevelFilter|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="LevelFilter">
    <xs:attribute name="LevelFilter" type="xs:string">
      <xs:annotation>
        <xs:documentation>Level at which ETW events should be filtered. All events at the same or lower level than the specified level are included.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="levelfilter"></a>LevelFilter
Poziom, na które zdarzenia ETW, które mają być filtrowane. Uwzględniane są wszystkie zdarzenia na poziomie sama lub starsza niż określony poziom.
|Atrybut|Wartość|
|---|---|
|name|LevelFilter|
|type|xs:String|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="LevelFilter" type="xs:string">
      <xs:annotation>
        <xs:documentation>Level at which ETW events should be filtered. All events at the same or lower level than the specified level are included.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="namevaluepair-attributegroup"></a>NameValuePair attributeGroup
Nazwa i wartość zdefiniowane jako atrybut.

|Atrybut|Wartość|
|---|---|
|content|atrybuty (2)|
|name|NameValuePair|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="NameValuePair">
    <xs:annotation>
      <xs:documentation>Name and Value defined as an attribute.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="Name" use="required">
      <xs:annotation>
        <xs:documentation>The name of the setting to override.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="Value" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>The new value of the setting.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="name"></a>Name (Nazwa)
Nazwa ustawienia do zastąpienia.
|Atrybut|Wartość|
|---|---|
|name|Name (Nazwa)|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Name" use="required">
      <xs:annotation>
        <xs:documentation>The name of the setting to override.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="value"></a>Wartość
Nowa wartość ustawienia.
|Atrybut|Wartość|
|---|---|
|name|Wartość|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Value" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>The new value of the setting.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="path-attributegroup"></a>Ścieżka attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|Ścieżka|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Path">
                <xs:attribute name="Path" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the file share. Format: file:[]</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        </xs:attributeGroup>
        
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="path"></a>Ścieżka
Ścieżka do udziału plików. Format: plik:]
|Atrybut|Wartość|
|---|---|
|name|Ścieżka|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Path" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the file share. Format: file:[]</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        
```

## <a name="relativefolderpath-attributegroup"></a>RelativeFolderPath attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|RelativeFolderPath|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RelativeFolderPath">
                <xs:attribute name="RelativeFolderPath" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the folder, relative to the application log directory.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        </xs:attributeGroup>
        
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="relativefolderpath"></a>RelativeFolderPath
Ścieżka do folderu, względem katalogu dziennika aplikacji.
|Atrybut|Wartość|
|---|---|
|name|RelativeFolderPath|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RelativeFolderPath" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the folder, relative to the application log directory.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        
```

## <a name="servicemanifestidentifier-attributegroup"></a>ServiceManifestIdentifier attributeGroup
Identyfikuje manifestu usługi.

|Atrybut|Wartość|
|---|---|
|content|atrybuty (2)|
|name|ServiceManifestIdentifier|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServiceManifestIdentifier">
    <xs:annotation>
      <xs:documentation>Identifies a service manifest.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="ServiceManifestName" use="required">
      <xs:annotation>
        <xs:documentation>The name of the service manifest. The name must match the Name declared in the ServiceManifest element of the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="ServiceManifestVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of the service manifest. The version must match the version declared in the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="servicemanifestname"></a>ServiceManifestName
Nazwa manifestu usługi. Nazwa musi odpowiadać nazwie zadeklarowany w elemencie ServiceManifest manifestu usługi.
|Atrybut|Wartość|
|---|---|
|name|ServiceManifestName|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServiceManifestName" use="required">
      <xs:annotation>
        <xs:documentation>The name of the service manifest. The name must match the Name declared in the ServiceManifest element of the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="servicemanifestversion"></a>ServiceManifestVersion
Wersja manifestu usługi. Wersja musi odpowiadać wersji zadeklarowane w manifeście usługi.
|Atrybut|Wartość|
|---|---|
|name|ServiceManifestVersion|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServiceManifestVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of the service manifest. The version must match the version declared in the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  
```

## <a name="uploadintervalinminutes-attributegroup"></a>UploadIntervalInMinutes attributeGroup
|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|UploadIntervalInMinutes|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="UploadIntervalInMinutes">
    <xs:attribute name="UploadIntervalInMinutes" type="xs:string">
      <xs:annotation>
        <xs:documentation>Interval in minutes at which data is uploaded to this destination.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="uploadintervalinminutes"></a>UploadIntervalInMinutes
Interwał w minutach, w których dane są przekazywane do tego miejsca docelowego.
|Atrybut|Wartość|
|---|---|
|name|UploadIntervalInMinutes|
|type|xs:String|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="UploadIntervalInMinutes" type="xs:string">
      <xs:annotation>
        <xs:documentation>Interval in minutes at which data is uploaded to this destination.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="versioneditemattrgroup-attributegroup"></a>VersionedItemAttrGroup attributeGroup
Grupa atrybutów na potrzeby przechowywania wersji sekcji w dokumentach ApplicationInstance i ServicePackage.

|Atrybut|Wartość|
|---|---|
|content|atrybuty (1)|
|name|VersionedItemAttrGroup|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="VersionedItemAttrGroup">
    <xs:annotation>
      <xs:documentation>Attribute group for versioning sections in ApplicationInstance and ServicePackage documents.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="RolloutVersion" type="xs:string" use="required"/>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="rolloutversion"></a>RolloutVersion
|Atrybut|Wartość|
|---|---|
|name|RolloutVersion|
|type|xs:String|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RolloutVersion" type="xs:string" use="required"/>
  
```

## <a name="versionedname-attributegroup"></a>VersionedName attributeGroup
Grupa atrybutów, która zawiera nazwę i wersję.

|Atrybut|Wartość|
|---|---|
|content|atrybuty (2)|
|name|VersionedName|

### <a name="xml-source"></a>Źródło XML
```xml
<xs:attributeGroup xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="VersionedName">
    <xs:annotation>
      <xs:documentation>Attribute group that includes a Name and a Version.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="Name" use="required">
      <xs:annotation>
        <xs:documentation>Name of the versioned item.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="Version" use="required">
      <xs:annotation>
        <xs:documentation>Version of the versioned item, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Szczegóły atrybutu

#### <a name="name"></a>Name (Nazwa)
Nazwa elementu wersjonowanego.
|Atrybut|Wartość|
|---|---|
|name|Name (Nazwa)|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Name" use="required">
      <xs:annotation>
        <xs:documentation>Name of the versioned item.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="version"></a>Wersja
Wersja elementu wersjonowanego ciągu bez struktury.
|Atrybut|Wartość|
|---|---|
|name|Wersja|
|Użyj|wymagane|
##### <a name="xml-source"></a>Źródło XML
```xml
<xs:attribute xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Version" use="required">
      <xs:annotation>
        <xs:documentation>Version of the versioned item, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  
```

