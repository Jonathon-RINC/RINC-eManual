---
layout: popup
---

- File : `std_msgs/msg/Int32MultiArray.msg`
- Raw Message Definition

  ```py
  # This was originally provided as an example message.
  # It is deprecated as of Foxy
  # It is recommended to create your own semantically meaningful message.
  # However if you would like to continue using this please use the equivalent in example_msgs.

  # Please look at the MultiArrayLayout message definition for
  # documentation on all multiarrays.

  MultiArrayLayout  layout        # specification of data layout
  int32[]           data          # array of data
  ```

- Compact Message Definition

  ```py
  std_msgs/msg/MultiArrayLayout layout
  int32[] data
  ```
