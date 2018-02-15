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

# mcumgr

MCU Manager (`mcumgr`) is the application tool that enables a user to communicate
with and manage remote devices running an
[mcumgr server](https://github.com/apache/mynewt-mcumgr).

The `mcumgr` tool is a thin wrapper over the Apache `newtmgr` tool.  Thus, the
[`newtmgr` documentation](http://mynewt.apache.org/latest/newtmgr/overview/)
provides some useful help with using the `mcumgr` tool.

### Building

Build the mcumgr tool as follows:

1. Unpack mcumgr source.
2. Rename resulting `apache-mynewt-mcumgr-1.3.0` directory to `$GOPATH/src/mynewt.apache.org/mcumgr`
3. `cd $GOPATH/src/mynewt.apache.org/mcumgr/mcumgr`
4. `go build`

### Examples

Here are some example `mcumgr` invocations.

#### Send an echo command over Bluetooth

The following sends an echo command to a Bluetooth device advertising the name
"Zephyr":

```
mcumgr --conntype ble --connstring peer_name=Zephyr echo hello
```

#### Upgrade firmware over Bluetooth

This series of commands performs an image upgrade over Bluetooth.  The device is assumed to be advertising the name "Zephyr".


```
# 1. Query device for its current image list.
mcumgr --conntype ble --connstring 'peer_name=Zephyr' image list

# 2. Upload new image to device.
mcumgr --conntype ble --connstring 'peer_name=Zephyr' image upload <filename>

# 3. Tell the device to run the new image on its next boot ("test" the new
#    image).
mcumgr --conntype ble --connstring 'peer_name=Zephyr' image test <image-hash>

# 4. Reboot the device.
mcumgr --conntype ble --connstring 'peer_name=Zephyr' reset

# 5. Query device for its current image list; ensure new image is running.
mcumgr --conntype ble --connstring 'peer_name=Zephyr' image list

# 6. Make the image swap permanent.
mcumgr --conntype ble --connstring 'peer_name=Zephyr' image confirm
```
