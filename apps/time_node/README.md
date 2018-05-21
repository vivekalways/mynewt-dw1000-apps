<!--
#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#  KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
#
-->

# Decawave TDMA based ranging application

## Overview
TDMA based ranging provides a slot based ranging. Here each tag will have a slot_id associated with it and uses its slot time 
to do ranging with required nodes. The CCP is used to acheive synchronization between different tags. The time module will
provide the methods to obtain the relative drift between the tag and the CCP master so that all the tags are now in same time
plane as that of master.

The user should add a time_postprocess function which calculates the transmission timestamp using its slot_id , slot_length and total slots available.

In the examples provided, time_tag will listen to the CCP packets & on reception of the CCP packets will callout the time_postprocess and the postprocess calculates the transmission timestamp & starts a delayed range request
& the time_node will be in always listening mode waiting for the range requests from different tags

1. Build the time_node & time_tag examples app

```no-highlight

newt target create tag0
newt target set tag0 app=apps/node_provision
newt target set tag0 bsp=@mynewt-dw1000-core/hw/bsp/dwm1001
newt target set tag0 build_profile=debug 
newt target amend tag0 syscfg=DEVICE_ID=0x1111
newt target amend tag0 syscfg=SLOT_ID=1
newt build tag0
newt load tag0
newt target amend tag0 syscfg=DEVICE_ID=0x1112
newt target amend tag0 syscfg=SLOT_ID=2
newt build tag0
newt load tag0
newt target amend tag0 syscfg=DEVICE_ID=0x1113
newt target amend tag0 syscfg=SLOT_ID=3
newt build tag0
newt load tag0
newt target amend tag0 syscfg=DEVICE_ID=0x1114
newt target amend tag0 syscfg=SLOT_ID=4
newt build tag0
newt load tag0

newt target create node0
newt target set node0 app=apps/tag_provision
newt target set node0 bsp=@mynewt-dw1000-core/hw/bsp/dwm1001
newt target set node0 build_profile=debug 
newt target amend node0 syscfg=DEVICE_ID=0xabab
newt build node0
newt load node0

```