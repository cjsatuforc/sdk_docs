<div style="float:right; padding:10px; margin-right:20px;"><a href="https://www.dronecode.org/sdk/"><img src="../assets/site/sdk_logo_full.jpg" title="Dronecode SDK Logo" width="400px"/></a></div>
# Dronecode SDK
[![Slack](https://px4-slack.herokuapp.com/badge.svg)](http://slack.px4.io)&nbsp;[![Discuss](https://img.shields.io/badge/discuss-DroneCore-ff69b4.svg)](http://discuss.px4.io/c/dronecore)  [![jenkins build status](http://ci.px4.io:8080/buildStatus/icon?job=DronecodeSDK/develop)](http://ci.px4.io:8080/blue/organizations/jenkins/DronecodeSDK/activity)
[![travis-ci build status](https://travis-ci.org/Dronecode/DronecodeSDK.svg?branch=develop)](https://travis-ci.org/Dronecode/DronecodeSDK)
[![appveyor build status](https://ci.appveyor.com/api/projects/status/1ntjvooywpxmoir8/branch/develop?svg=true)](https://ci.appveyor.com/project/julianoes/dronecore/branch/develop)
[![Coverage Status](https://coveralls.io/repos/github/Dronecode/DronecodeSDK/badge.svg?branch=develop)](https://coveralls.io/github/Dronecode/DronecodeSDK?branch=develop)

The *Dronecode SDK* is a [MAVLink](https://mavlink.io/en/) Library for the [PX4 flight stack](http://px4.io), with APIs for C++, Python, Android, and iOS (coming soon). 

> **Tip** The SDK is the best way to integrate with PX4 over MAVLink! 
  It is supported by [Dronecode](https://www.dronecode.org/), ensuring that it is robust, well tested, and maintained. 

The library provides a simple core C++ API for managing one or more vehicles, providing programmatic access to vehicle information and telemetry, and control over missions, movement and other operations.

Developers can extend the SDK using plugins in order to add any other required MAVLink API (for example, to integrate PX4 with custom cameras, gimbals, or other hardware over MAVLink).

The library can run on a vehicle-based companion computer or on a ground-based GCS or mobile device. 
These devices have significantly more processing power that an ordinary flight controller, enabling tasks like computer vision, obstacle avoidance, and route planning.

## Project Status

The SDK is still in alpha development. 
- The core C++ API has been created and is (largely) stable.
- Currently you can only develop in C++. 
  Cross-platform wrappers are being developed using [gRPC](https://grpc.io/) and [Reactive Extensions](http://reactivex.io/).

To use the SDK you will need to [build the C++ library](contributing/build.md). 
The [Guide](guide/README.md) explains how to write SDK apps using C++. 
A number complete examples can be found [here](examples/README.md).


## Library Features

The core library is written in C++, with auto-generated bindings for other supported programming languages.

The library is:
- Straightforward and easy to use. It has an API that supports both synchronous (blocking) and asynchronous calls (using callbacks). 
- Fast, robust, and lightweight. Built to handle onboard usage with high rate messaging.
- Cross-platform (Linux, macOS, Windows, iOS, Android).
- Extensible, using compile-time plugins.

The main features provided by the core API are (in all programing languages):

* Connect to and manage up to 255 vehicles via a TCP, UDP or serial connection.
* Get information about vehicles (vendor, software versions, product versions etc.)
* Get vehicle telemetry and state information (e.g. battery, GPS, RC connection, flight mode etc.) and set telemetry update rates.
* Send commands to arm, disarm, kill, takeoff, land and return to launch.
* Create and manage missions.
* Control a camera and gimbal both inside and outside of missions.
* Send commands to directly control vehicle movement.
* Send commands to start sensor calibration.

See the [FAQ](getting_started/faq.md) for answers to common questions about the library. 


## API Overview

[DronecodeSDK](/api_reference/classdronecode__sdk_1_1_dronecode_s_d_k.md) is the main library class. API consumers use [DronecodeSDK](/api_reference/classdronecode__sdk_1_1_dronecode_s_d_k.md) to discover and access vehicles ([System](/api_reference/classdronecode__sdk_1_1_system.md) objects), which in turn provide access to all other drone information and control objects (e.g. [Telemetry](/api_reference/classdronecode__sdk_1_1_telemetry.md), [Mission](/api_reference/classdronecode__sdk_1_1_mission.md) etc.).

The most important classes are:

- [DronecodeSDK](/api_reference/classdronecode__sdk_1_1_dronecode_s_d_k.md): Discover and connect to vehicles ([System](/api_reference/classdronecode__sdk_1_1_system.md)).
- [System](/api_reference/classdronecode__sdk_1_1_system.md): Represents a connected vehicle (e.g. a copter or VTOL drone). It provides access to vehicle information and control through the classes listed below.
- [Info](/api_reference/classdronecode__sdk_1_1_info.md): Basic version information about the hardware and/or software of a system.
- [Telemetry](/api_reference/classdronecode__sdk_1_1_telemetry.md): Get vehicle telemetry and state information ([Battery](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_battery.md), [EulerAngle](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_euler_angle.md), [GPSInfo](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_g_p_s_info.md), [GroundSpeedNED](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_ground_speed_n_e_d.md), [Health](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_health.md), [Position](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_position.md), [Quaternion](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_quaternion.md), [RCStatus](/api_reference/structdronecode__sdk_1_1_telemetry_1_1_r_c_status.md)) and set telemetry update rates.
- [Action](/api_reference/classdronecode__sdk_1_1_action.md): Simple drone actions including arming, taking off, and landing.
- [Mission](/api_reference/classdronecode__sdk_1_1_mission.md): Waypoint mission creation and upload/download. Missions are created from [MissionItem](/api_reference/classdronecode__sdk_1_1_mission_item.md) objects.
- [Offboard](/api_reference/classdronecode__sdk_1_1_offboard.md): Control a drone with velocity commands.
- [Gimbal](/api_reference/classdronecode__sdk_1_1_gimbal.md): Control a gimbal.
- [Camera](/api_reference/classdronecode__sdk_1_1_camera.md): Control a camera.
- [FollowMe](/api_reference/classdronecode__sdk_1_1_follow_me.md): Drone tracks a position supplied by the SDK.
- [Calibration](/api_reference/classdronecode__sdk_1_1_calibration.md):  Calibrate sensors (e.g.: gyro, accelerometer, and magnetometer).
- [Logging](/api_reference/classdronecode__sdk_1_1_logging.md): Data logging and streaming from the vehicle.


## Getting Started

The [Quick Start](getting_started/README.md) explains how to set up a development environment and write a minimal example for all our supported programming languages/platforms. 

Developers who want to contribute to the API will need to build the C++ library (and other programming language wrappers) from source. For more information see the [contributing section](#contributing) below.
 

## Getting Help

This guide contains information and examples showing how to use the SDK. 
If you have specific questions that are not answered by the documentation, these can be raised on:

* [Discuss board](http://discuss.px4.io/c/sdk)
* [Slack (#sdk)](https://px4.slack.com/messages/C68J8H32A) (get a [Slack login here](http://slack.px4.io))

Use Github for bug reports/enhancement requests:

* [C++ API](https://github.com/Dronecode/DronecodeSDK/issues)
* [Documentation](https://github.com/dronecore/sdk_docs/issues)
<!-- Add info about where Python etc API issues are reported). -->

## Contributing

We welcome contributions! If you want to help or have suggestions/bug reports [please get in touch with the development team](#getting-help). 

The [Contributing](contributing/README.md) section contains everything you need to contribute, including topics about building the SDK from source code, running our integration and unit tests, and all other aspects of core development. 


## License

* The *Dronecode SDK* is licensed under the permissive [BSD 3-clause](https://github.com/Dronecode/DronecodeSDK/blob/{{ book.github_branch }}/LICENSE.md).
* This documentation is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.
