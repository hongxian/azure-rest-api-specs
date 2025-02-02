# Maps

> see https://aka.ms/autorest

This is the AutoRest configuration file for Maps.

---

## Getting Started

To build the SDK for Maps, simply [Install AutoRest](https://aka.ms/autorest/install) and in this folder, run:

> `autorest`

To see additional help and options, run:

> `autorest --help`

---

## Configuration

### Basic Information

These are the global settings for the Maps API.

``` yaml
openapi-type: arm
tag: package-preview-2020-02
```

### Tag: package-preview-2020-02

These settings apply only when `--tag=package-preview-2020-02` is specified on the command line.

``` yaml $(tag) == 'package-preview-2020-02'
input-file:
  - Microsoft.Maps/preview/2020-02-01-preview/maps-management.json
```

### Tag: package-2017-01

These settings apply only when `--tag=package-2017-01` is specified on the command line.

``` yaml $(tag) == 'package-2017-01'
input-file:
- Microsoft.Maps/stable/2017-01-01-preview/maps-management.json
```

### Tag: package-2018-05

These settings apply only when `--tag=package-2018-05` is specified on the command line.

``` yaml $(tag) == 'package-2018-05'
input-file:
- Microsoft.Maps/stable/2018-05-01/maps-management.json
```

---

# Code Generation

## Swagger to SDK

This section describes what SDK should be generated by the automatic system.
This is not used by Autorest itself.

``` yaml $(swagger-to-sdk)
swagger-to-sdk:
  - repo: azure-sdk-for-net
  - repo: azure-sdk-for-python
  - repo: azure-sdk-for-python-track2
  - repo: azure-sdk-for-go
  - repo: azure-sdk-for-js
  - repo: azure-sdk-for-node
  - repo: azure-resource-manager-schemas
    after_scripts:
      - node sdkauto_afterscript.js maps/resource-manager
```

## C#

These settings apply only when `--csharp` is specified on the command line.
Please also specify `--csharp-sdks-folder=<path to "SDKs" directory of your azure-sdk-for-net clone>`.

``` yaml $(csharp)
csharp:
  azure-arm: true
  license-header: MICROSOFT_MIT_NO_VERSION
  namespace: Microsoft.Azure.Management.Maps
  output-folder: $(csharp-sdks-folder)/maps/Microsoft.Azure.Management.Maps/src/Generated
  clear-output-folder: true
```

## Python

See configuration in [readme.python.md](./readme.python.md)

## Go

See configuration in [readme.go.md](./readme.go.md)

## Java

These settings apply only when `--java` is specified on the command line.
Please also specify `--azure-libraries-for-java-folder=<path to the root directory of your azure-libraries-for-java clone>`.

``` yaml $(java)
azure-arm: true
fluent: true
namespace: com.microsoft.azure.management.maps
license-header: MICROSOFT_MIT_NO_CODEGEN
payload-flattening-threshold: 1
output-folder: $(azure-libraries-for-java-folder)/azure-mgmt-maps
```

### Java multi-api

``` yaml $(java) && $(multiapi)
batch:
  - tag: package-2017-01
  - tag: package-2018-05
  - tag: package-2020-02
```

### Tag: package-2020-02 and java

These settings apply only when `--tag=package-2020-02 --java` is specified on the command line.
Please also specify `--azure-libraries-for-java=<path to the root directory of your azure-sdk-for-java clone>`.

``` yaml $(tag) == 'package-2020-02' && $(java) && $(multiapi)
java:
  namespace: com.microsoft.azure.management.maps.v2020_02_01_preview
  output-folder: $(azure-libraries-for-java-folder)/sdk/maps/mgmt-v2020_02_01_preview
regenerate-manager: true
generate-interface: true
```

### Tag: package-2017-01 and java

These settings apply only when `--tag=package-2017-01 --java` is specified on the command line.
Please also specify `--azure-libraries-for-java=<path to the root directory of your azure-sdk-for-java clone>`.

``` yaml $(tag) == 'package-2017-01' && $(java) && $(multiapi)
java:
  namespace: com.microsoft.azure.management.maps.v2017_01_01_preview
  output-folder: $(azure-libraries-for-java-folder)/sdk/maps/mgmt-v2017_01_01_preview
regenerate-manager: true
generate-interface: true
```

### Tag: package-2018-05 and java

These settings apply only when `--tag=package-2018-05 --java` is specified on the command line.
Please also specify `--azure-libraries-for-java=<path to the root directory of your azure-sdk-for-java clone>`.

``` yaml $(tag) == 'package-2018-05' && $(java) && $(multiapi)
java:
  namespace: com.microsoft.azure.management.maps.v2018_05_01
  output-folder: $(azure-libraries-for-java-folder)/sdk/maps/mgmt-v2018_05_01
regenerate-manager: true
generate-interface: true
```

## Suppression

``` yaml
directive:
  - suppress: R2017
    where:
      - '$.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Maps/accounts/{accountName}/privateAtlases/{privateAtlasName}"].put'
      - '$.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Maps/accounts/{accountName}"].put'
    from: maps-management.json
    reason:
      - Common type models are inherited.
      - ClientId property will be ignored by requests
  - suppress: R2001
    where:
      - $.definitions.PrivateAtlas.properties.properties
      - $.definitions.MapsAccount.properties.properties
    from: maps-management.json
    reason:
      - Flattening does not work well with polymorphic models.
      - PrivateAtlas.properties is an arbitrary dictionary and cannot be flattened.
      - MapsAccount.properties is an arbitrary dictionary and cannot be flattened.
  - suppress: R3006
    where:
      - $.definitions.MapsAccount.properties
    reason:
      - Currently systemData is not allowed.
  - suppress: R3010
    where:
      - $.definitions
    reason:
      - Pipeline runs are not listable. The operation PrivateAtlases_ListByAccount serves this purpose.
  - suppress: R3027
    where:
      - $.definitions.PrivateAtlas
    reason:
      - This is a nested tracked resource.
  - suppress: R3028
    where:
      - $.definitions.PrivateAtlas
    reason:
      - This is a nested tracked resource.
  - suppress: R4000
    where:
      - $.definitions.Resource
    from: types.json
    reason:
      - Common type models are inherited.
  - suppress: AvoidNestedProperties
    where: $.definitions.Creator.properties.properties
    from: maps-management.json
    reason: False positive.
  - suppress: TrackedResourceListByResourceGroup
    where: $.definitions.Creator
    from: maps-management.json
    reason: Don't need to list by resourceGroup because this is a nested resource.
  - suppress: TrackedResourceListBySubscription
    where: $.definitions.Creator
    from: maps-management.json
    reason: Doesn't apply because this is a nested resource.
  - suppress: PutRequestResponseScheme
    where: '$.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Maps/accounts/{accountName}/creators/{creatorName}"].put'
    from: maps-management.json
    reason: False positive. Structure is the same with addition of provisioningStatus property.
```

## AzureResourceSchema

See configuration in [readme.azureresourceschema.md](./readme.azureresourceschema.md)

