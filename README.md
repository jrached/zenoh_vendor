This repo was forked from https://github.com/pac-ws/zenoh_vendor

**Build**  
Note: the zenoh bridge is prebuilt. You can simply unzip the binary and run it in the workspace. The cmake file just automates this.
```bash
colcon build --packages-select zenoh_vendor
```

**Run**  
The bridge takes an optional config (-c) that allows the specification of key parameters, such as topics for the bridge and mode selection.
```bash
ros2 run zenoh_vendor zenoh-bridge-ros2dds -c <file_name>
```

On the Ground Control Station (GCS) pc:
```bash
ros2 run zenoh_vendor -c /workspace/src/zenoh_vendor/configs/zenoh_gcs.json5
```

On the the vehicle:
```bash
ros2 run zenoh_vendor -c zenoh-bridge-ros2dds /workspace/src/zenoh_vendor/configs/zenoh_agent.json5
```

**Notes** 
1. The network is setup as a server-client architecture with the base station as the server and the agents as the clients. The base station router must be run before the agent routers.
2. You can modify the config files to enable/disable the publishing/subscription of any topic by name.
3. CycloneDDS must be used as the local ROS2 middleware, while Zenoh will run accross devices.
4.  In your bashrc the following ROS2 environment variables must be configured:
   ```bash
   RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
   ROS_DOMAIN_ID=10
   ROS_AUTOMATIC_DISCOVERY_RANGE=LOCALHOST
   ```
5. It is crucial that the native ROS2 middleware, in this case CycloneDDS, is not configured to use the device's wifi port. If it is, both Zenoh and CycloneDDS will run across the network, which will cause undesired behavior - such as disabled topics leaking through and publishing loopback (topics publish at > 1000 Hz frequencies).
6. The Zenoh bridge has a maximum throughput of about 5 Mb/s. If any topic exceeds that limit, its publishing frequency will be reduced to accomodate the throughput cap. 

