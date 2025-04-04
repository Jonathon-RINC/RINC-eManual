---
layout: archive
lang: en
ref: matlab_groupbulkwrite
read_time: true
share: true
author_profile: false
permalink: /docs/en/software/dynamixel/dynamixel_sdk/api_reference/matlab/matlab_groupbulkwrite/
sidebar:
  title: DYNAMIXEL SDK
  nav: "dynamixel_sdk"
---

<style>body {counter-reset: h1 6 !important;}</style>
<div style="counter-reset: h2 6"></div>
<div style="counter-reset: h3 4"></div>

<!--[dummy Header 1]>
  <h1 id="api-reference"><a href="#api-reference">API Reference</a></h1>
  <h2 id="matlab"><a href="#matlab">Matlab</a></h2>
<![end dummy Header 1]-->

### [Matlab GroupBulkWrite](#matlab-groupbulkwrite)

- Description

  Base functions for simultaneous dynamixel control on writing to different length data on different control table address.

- Members

  None


- Methods

| Methods                                                     | Description                                       |
|:------------------------------------------------------------|:--------------------------------------------------|
| **[groupBulkWrite](#groupbulkwrite)**                       | Initializes members of packet data pointer struct |
| **[groupBulkWriteAddParam](#groupbulkwriteaddparam)**       | Adds parameter storage for read                   |
| **[groupBulkWriteRemoveParam](#groupbulkwriteremoveparam)** | Removes parameter on the storage                  |
| **[groupBulkWriteChangeParam](#groupbulkwritechangeparam)** | Changes parameter on the storage                  |
| **[groupBulkWriteClearParam](#groupbulkwriteclearparam)**   | Clears parameter storage                          |
| **[groupBulkWriteTxPacket](#groupbulkwritetxpacket)**       | Transmits packet to the number of DYNAMIXEL's      |

- Enumerator

  None

#### Method References

##### groupBulkWrite
- Syntax
```c
int groupBulkWrite(int port_num, int protocol_version)
```
- Parameters

| Parameters       | Description      |
|:-----------------|:-----------------|
| port_num         | Port number      |
| protocol_version | Protocol version |

- Detailed Description

   This function initializes the parameters for packet construction. The function resizes groupData struct and initialzes struct members.

##### groupBulkWriteAddParam
- Syntax
```c
bool groupBulkWriteAddParam(int group_num, int id, int start_address, int data_length, int data, int input_length)
```
- Parameters

| Parameters    | Description       |
|:--------------|:------------------|
| group_num     | Group number      |
| id            | DYNAMIXEL ID      |
| start_address | Start address     |
| data_length   | Data length       |
| data          | Data for write    |
| input_length  | Input data length |

- Detailed Description

    This function pushes `id` into the DYNAMIXEL ID list, and initializes #`group_num` parameter storage by setting `start_address` and `param_length`. The function deals with 1, 2 or 4 byte data only. The function returns false when the length of input data exceeds parameter or wrong length, or returns true.   


##### groupBulkWriteRemoveParam
- Syntax
```c
void groupBulkWriteRemoveParam(int group_num, int id)
```
- Parameters

| Parameters | Description  |
|:-----------|:-------------|
| group_num  | Group number |
| id         | DYNAMIXEL ID |

- Detailed Description

   This function removes `id` and its data for write in the #`group_num` DYNAMIXEL ID list. The function returns false when no data is in the parameter storage, or returns true.


##### groupBulkWriteChangeParam
- Syntax
```c
 bool groupBulkWriteChangeParam(int group_num, int id, int start_address, int data_length, int data, int input_length, int data_pos)
```
- Parameters

| Parameters    | Description                |
|:--------------|:---------------------------|
| group_num     | Group number               |
| id            | DYNAMIXEL ID               |
| start_address | Start address              |
| data_length   | Data length                |
| data          | data for write             |
| input_length  | Input data length          |
| data_pos      | Data position in the array |

- Detailed Description

   This function pushes new `data_length` bytes data into #`group_num` parameter storage of same id. The function returns false when the target ID doesn’t exists in the ID list, or returns true.

##### groupBulkWriteClearParam
- Syntax
```c
void groupBulkWriteClearParam(int group_num)
```
- Parameters

| Parameters | Description  |
|:-----------|:-------------|
| group_num  | Group number |

- Detailed Description

   This function clears #`group_num` DYNAMIXEL ID list.


##### groupBulkWriteTxPacket
- Syntax
```c
int groupBulkWriteTxPacket(int group_num)
```
- Parameters

| Parameters | Description  |
|:-----------|:-------------|
| group_num  | Group number |

- Detailed Description

   This function transmits the packet by using `BulkWriteTxOnly` function. The communication result and the hardware error are available when the function is terminated.
