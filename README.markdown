[![Build Status](https://travis-ci.org/autostructure/artifactory_pro.svg?branch=master)](https://travis-ci.org/autostructure/artifactory_pro)
[![Puppet Forge](https://img.shields.io/puppetforge/v/autostructure/artifactory_pro.svg)](https://forge.puppetlabs.com/autostructure/artifactory_pro)
[![Puppet Forge](https://img.shields.io/puppetforge/f/autostructure/artifactory_pro.svg)](https://forge.puppetlabs.com/autostructure/artifactory_pro)

#### Table of Contents

1. [Overview](#overview)
2. [Module Description - What the module does and why it is useful](#module-description)
3. [Setup - The basics of getting started with artifactory](#setup)
    * [Setup requirements](#setup-requirements)
    * [Beginning with artifactory](#beginning-with-artifactory)
4. [Usage - Configuration options and additional functionality](#usage)
5. [Reference - An under-the-hood peek at what the module is doing and how](#reference)
5. [Limitations - OS compatibility, etc.](#limitations)
6. [Development - Guide for contributing to the module](#development)

## Overview

This ONLY install Artifactory Pro.

If you are looking for the commercial installation look at:

Artifactory OSS: https://forge.puppet.com/autostructure/artifactory

Artifactory HA: https://forge.puppet.com/autostructure/artifactory_ha

Github and gitlab are great for storing source control, but bad at storing installers and compiled packages.

This is where Artifactory comes in. It stores all of your organizations artifacts in an organized and secure manner.

## Module Description

The Artifactory Pro module installs, configures, and manages the Artifactory pro binary repository.

The Artifactory Pro module manages both the installation and database configuration of Artifactory Pro.

## Setup

### Beginning with artifactory

Artifactory pro requires at a minimum a license key

~~~
class { '::artifactory_pro':
  license_key                    => 'abc123',
}
~~~

If you need to add database connectivity instantiate with the required parameters:

~~~
class { '::artifactory_pro':
  license_key                    => 'abc123',
  jdbc_driver_url                => 'puppet://modules/my_module/mysql.jar',
  db_type                        => 'oracle',
  db_url                         => 'jdbc:oracle:thin:@somedomain.com:1521:arti001',
  db_username                    => 'my_username',
  db_password                    => 'efw23gn2j3',
  binary_provider_type           => 'filesystem',
  pool_max_active                => 100,
  pool_max_idle                  => 10,
  binary_provider_cache_maxSize  => $binary_provider_cache_maxSize,
  binary_provider_filesystem_dir => '/var/opt/jfrog/artifactory/data/filestore',
  binary_provider_cache_dir      => '/var/opt/jfrog/artifactory/',
}
~~~

## Usage

All interaction for the server is done via `::artifactory_pro`.

## Reference

### Classes

#### Public classes

* [`artifactory`](#artifactoryserver): Installs and configures Artifactory.

#### Private classes

* `artifactory_pro::config`: Configures Artifactory Pro.
* `artifactory_pro::post_config`: Does pro post configuration.

### Defines

#### Public defines

* `artifactory_pro::plugin`: Adds a groovy plugin to the server

### Parameters

#### artifactory

##### `license_key`

Sets the name of the Artifactory license key'.

This is required.

##### `yum_name`

Sets the name of the yum repository. Defaults to 'bintray-jfrog-artifactory-pro-rpms'.

This can be changed if Artifactory needs to be setup from a different repository. Typically this is done if an organization has a 'trusted' yum repo.

##### `yum_baseurl`

Sets the base url of the yum repository to name. Defaults to 'http://jfrog.bintray.com/artifactory-pro-rpms'.

This can be changed if Artifactory needs to be setup from a different repository. Typically this is done if an organization has a 'trusted' yum repo.

##### `package_name`

Sets the package name to install. Defaults to 'jfrog-artifactory-pro'.

This can be changed if Artifactory needs to install a differently named package. Possibly needed if na organization creates their own Artifactory package.

##### `manage_java`

Tells the module whether or not to manage the java class. This defaults to true. Usually this is what you want.

If your organization actively manages the java installs across your environment set this to false.

##### `jdbc_driver_url`

Sets the location for the jdbc driver. Uses the wget module to retrieve the driver.

This is required if using a new data source.

##### `db_type`

Only required for database configuration. The type of database to configure for. Valid values are 'mssql', 'mysql', 'oracle', 'postgresql'.

##### `db_url`

Only required for database configuration. The url of the database.

##### `db_username`

Only required for database configuration. The username for the database account.

##### `db_password`

Only required for database configuration. The password for the database account.

##### `binary_provider_type`

Optional setting for the binary storage provider. The type of database to configure for. Valid values are 'filesystem', 'fullDb', 'cachedFS', 'S3'. Defaults to 'filesystem'.

###### filesystem (default)
This means that metadata is stored in the database, but binaries are stored in the file system. The default location is under $ARTIFACTORY_HOME/data/filestore however this can be modified.

###### fullDb
All the metadata and the binaries are stored as BLOBs in the database.

###### cachedFS
Works the same way as filesystem but also has a binary LRU (Least Recently Used) cache for upload/download requests. Improves performance of instances with high IOPS (I/O Operations) or slow NFS access.

###### clusterGCS
This is the setting used for clustered GCS storage.

###### S3
This is the setting used for S3 Object Storage.

##### `pool_max_active`

Optional setting for the maximum number of pooled database connections. Defaults to 100.

##### `pool_max_idle`

Optional setting for the maximum number of pooled idle database connections Defaults to 10.

##### `binary_provider_cache_maxSize`

Optional setting for the maximum cache size. This value specifies the maximum cache size (in bytes) to allocate on the system for caching BLOBs.

##### `binary_provider_filesystem_dir`

Optional setting for the artifactory filestore location. The binary.provider.type is set to filesystem this value specifies the location of the binaries. Defaults to '$ARTIFACTORY_HOME/data/filestore'.

##### `binary_provider_cache_dir`

Optional setting for the location of the cache. This should be set to your $ARTIFACTORY_HOME directory directly (not on the NFS).

##### `binary_provider_credential`

Optional setting for clusterGCS filestore. Your cloud storage provider authentication credential.

##### `binary_provider_identity`

Optional setting for clusterGCS filestore. Your cloud storage provider identity.

##### `binary_provider_bucket`

Optional setting for clusterGCS filestore.

#### artifactory::plugin

##### `url`

The url to the location of the groovy file.

## Limitations

This module has been tested on:

* RedHat Enterprise Linux 5, 6, 7
* CentOS 5, 6, 7

## Development

Since your module is awesome, other users will want to play with it. Let them know what the ground rules for contributing are.

### Authors

This module is based on work by Autostructure. The following contributors have contributed to this module:

* Bryan Belanger
