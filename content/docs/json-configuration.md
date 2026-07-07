+++
title = 'JSON Configuration'
weight = 2
+++

October Linux provides an installer that takes a JSON configuration file to deploy your system.
The JSON defines things like disk partitioning, users and extra packages you want installed.

This page goes over all you need to know to prepare your JSON configuration to deploy a fully operational system.

## Full JSON example

```json
{
  "drives" : [
    {
      "path": "/dev/xyz",
      "append": true/false,
      "partitions": [
        {
          "size": {
            "amount": 1234,
            "unit": "MiB/GiB/etc.",
            "takeRemaining": true/false
          },
          "fileSystem": "btrfs/ext4",
          "partitionType": "gpt partition type (guid)",
          "mountPoint": "/absolute/path/to/directory"
        }
      ]
    }
  ],
  "users": [
    {
      "username": "[username]",
      "password": "[password]",
      "homepath": "[path to home]",
      "sudoer": true
    }
  ],
  "mirrorCountries": [
    "list of countries"
  ],
  "timezone": "[user timezone]",
  "locale": "[user locale]",
  "hostname": "[user hostname]",
  "rootPassword": "[root password]",
  "bestEffortGPU": true,
  "extraPackages": {
    "officialRepositories": ["package1", "package2"],
    "aur": ["package1", "package2"]
  }
}
```

## Drives

Drives contains an array of "drives" and those contain partitions. This section dictates what partitions to create on which drives.

### Example

A basic drive configuration looks like this:
```json
"drives": [
  {
    "path": "/dev/sda",
    "append": false,
    "partitions": [
      {
        "size": {
          "amount": 1,
          "unit": "GiB"
        },
        "partitionType": "C12A7328-F81F-11D2-BA4B-00A0C93EC93B"
      },
      {
        "size": {
          "amount": 4,
          "unit": "GiB"
        },
        "partitionType": "0657FD6D-A4AB-43C4-84E5-0933C84B4F4F"
      },
      {
        "size": {
          "takeRemaining": true
        },
        "partitionType": "4F68BCE3-E8CD-4DB1-96E7-FBCAF984B709",
        "fileSystem": "ext4"
      }
    ]
  }
],
```

### Supported partition table

Currently, the installer **only supports GPT drives**, so make sure to format drives on which you'll create partitions to GPT
before running the installer.

This can be done by doing:
```shell
$ fdisk /dev/sda
Command (m for help): g
Command (m for help): w
```

### Supported partition types

The installer currently support all the listed partition types below. The ones with an asterisk are **needed** for the system to be functional.

| Name | GUID | Description |
| ----- | ---- | ---- |
| EFI* | C12A7328-F81F-11D2-BA4B-00A0C93EC93B | Used for the booloader to boot the system. |
| SWAP* | 0657FD6D-A4AB-43C4-84E5-0933C84B4F4F | Systems swap space. |
| Root* | 4F68BCE3-E8CD-4DB1-96E7-FBCAF984B709 | The partition that will have root (/). |
| File System | 0FC63DAF-8483-4772-8E79-3D69D8477DE4 | Partition with data on it. |
| Home | 933AC7E1-2EB4-4F13-B844-0E14E2AEF915 | Optional partition for home directory. |

### Supported file systems

The installer currently support two file systems:
- btrfs
- ext4

### Drive, partition and size keys

This is a table of all the keys and descriptions for each of them.
 
#### Drive keys
| Name | type | Description | Needed |
| --- | --- | --- | --- |
| path | string | The absolute path to the drive. | Yes |
| append | boolean | Whether the new partitions will be appended to the existing partition table or replace it. | Yes |
| partitions | array of [objects](#partition-keys) | Array of all the partitions that need to be created. | Yes |

#### Partition keys 
| Name | type | Description | Needed |
| --- | --- | --- | --- |
| size | [object](#size-keys) | The size of the new partition. | Yes |
| partitionType | string | The GUID of the partition type. | Yes |
| fileSystem | string | The partition file system. | If partitionType is not EFI or SWAP. |
| mountPoint | string | The mount point of the drive | If partitionType is not EFI, SWAP or Root. |


#### Size keys
| Name | type | Description | Needed |
| --- | --- | --- | --- |
| amount | integer | The size of the new drive. | If takeRemaining is **false** or **not present**. |
| unit | string | The unit of the given amount. Units are in *iB (like GiB).| If amount is specified. |
| takeRemaining | boolean | If true, it will take the remaining space of the drive for this partition. | If no amount is specified. |

## User

Users contains an array of users that needs to be created after the installation.

### Users configuration example
```json
{
  "username": "testuser",
  "password": "test",
  "homepath": "/home/testuser",
  "sudoer": true
}
```

### User keys

| Name | Type | Description | Needed |
| --- | --- | --- | --- |
| username | string | The username of the user. | Yes |
| password | string | The password of the user. | Yes |
| homepath | string | The absolute path to the user home folder. | No. Default: /home/[username] |
| sudoer | boolean | Is the user a sudoer on the new install. | Yes |

## General configuration

This part contains all the more "general" configuration of the installation.

### General configuration example
```json
{
  "mirrorCountries": ["Canada"],
  "timezone": "America/Montreal",
  "locale": "US.UTF-8",
  "hostname": "testhostname",
  "rootPassword": "test",
  "bestEffortGPU": false,
  "extraPackages": {
    "officialRepositories": ["package1", "package2"],
    "aur": ["package1", "package2"]
  }
}
```

### General configuration keys

| Name | Type | Description | Needed |
| --- | --- | --- | --- |
| mirrorCountries | array of strings | Names of the countries you want to use mirrors from. They can be seen on the [Arch Wiki](https://archlinux.org/mirrorlist/all/https/).| Yes |
| timezone | string | The timezone you want the system to be set to. | Yes |
| locale | string | The locale you want to set on the system. Only UTF-8 locales are supported. | Yes |
| hostname | string | The hostname of the new system. | Yes |
| rootPassword | string | The root password on the new install. | Yes |
| bestEffortGPU | boolean | If true, it will attempt to install the drivers for the systems GPU on the new install. Only Nvidia, AMD and Intel are supported. | Yes |
| extraPackages | object | The extra packages to be installed on the system. | No |
| extraPackages.officialRepositories | array of strings | The extra packages to be installed from the official Arch Linux repositories | No |
| extraPackages.aur | array of strings | The extra packages to be installed from the Arch User Repository (AUR) | No |
