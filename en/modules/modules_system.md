# Modules Reference: System

## battery_simulator
Source: [modules/simulation/battery_simulator](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/simulation/battery_simulator)


### Description



<a id="battery_simulator_usage"></a>
### Usage
```
battery_simulator <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## battery_status
Source: [modules/battery_status](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/battery_status)


### Description

The provided functionality includes:
- Read the output from the ADC driver (via ioctl interface) and publish `battery_status`.


### Implementation
It runs in its own thread and polls on the currently selected gyro topic.


<a id="battery_status_usage"></a>
### Usage
```
battery_status <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## camera_feedback
Source: [modules/camera_feedback](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/camera_feedback)


### Description



<a id="camera_feedback_usage"></a>
### Usage
```
camera_feedback <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## commander
Source: [modules/commander](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/commander)


### Description
The commander module contains the state machine for mode switching and failsafe behavior.

<a id="commander_usage"></a>
### Usage
```
commander <command> [arguments...]
 Commands:
   start
     [-h]        Enable HIL mode

   calibrate     Run sensor calibration
     mag|baro|accel|gyro|level|esc|airspeed Calibration type
     quick       Quick calibration (accel only, not recommended)

   check         Run preflight checks

   arm
     [-f]        Force arming (do not run preflight checks)

   disarm
     [-f]        Force disarming (disarm in air)

   takeoff

   land

   transition    VTOL transition

   mode          Change flight mode
     manual|acro|offboard|stabilized|altctl|posctl|auto:mission|auto:loiter|auto
                 :rtl|auto:takeoff|auto:land|auto:precland Flight mode

   pair

   lockdown
     on|off      Turn lockdown on or off

   set_ekf_origin
     lat, lon, alt Origin Latitude, Longitude, Altitude

   lat|lon|alt   Origin latitude longitude altitude

   poweroff      Power off board (if supported)

   stop

   status        print status info
```
## dataman
Source: [modules/dataman](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/dataman)


### Description
Module to provide persistent storage for the rest of the system in form of a simple database through a C API.
Multiple backends are supported:
- a file (eg. on the SD card)
- RAM (this is obviously not persistent)

It is used to store structured data of different types: mission waypoints, mission state and geofence polygons.
Each type has a specific type and a fixed maximum amount of storage items, so that fast random access is possible.

### Implementation
Reading and writing a single item is always atomic. If multiple items need to be read/modified atomically, there is
an additional lock per item type via `dm_lock`.

**DM_KEY_FENCE_POINTS** and **DM_KEY_SAFE_POINTS** items: the first data element is a `mission_stats_entry_s` struct,
which stores the number of items for these types. These items are always updated atomically in one transaction (from
the mavlink mission manager). During that time, navigator will try to acquire the geofence item lock, fail, and will not
check for geofence violations.


<a id="dataman_usage"></a>
### Usage
```
dataman <command> [arguments...]
 Commands:
   start
     [-f <val>]  Storage file
                 values: <file>
     [-r]        Use RAM backend (NOT persistent)

 The options -f and -r are mutually exclusive. If nothing is specified, a file
 'dataman' is used

   stop

   status        print status info
```
## dmesg
Source: [systemcmds/dmesg](https://github.com/PX4/PX4-Autopilot/tree/main/src/systemcmds/dmesg)


### Description

Command-line tool to show bootup console messages.
Note that output from NuttX's work queues and syslog are not captured.

### Examples

Keep printing all messages in the background:
```
dmesg -f &
```

<a id="dmesg_usage"></a>
### Usage
```
dmesg <command> [arguments...]
 Commands:
     [-f]        Follow: wait for new messages
```
## esc_battery
Source: [modules/esc_battery](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/esc_battery)


### Description
This implements using information from the ESC status and publish it as battery status.


<a id="esc_battery_usage"></a>
### Usage
```
esc_battery <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## gyro_calibration
Source: [modules/gyro_calibration](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/gyro_calibration)


### Description
Simple online gyroscope calibration.


<a id="gyro_calibration_usage"></a>
### Usage
```
gyro_calibration <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## gyro_fft
Source: [modules/gyro_fft](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/gyro_fft)


### Description


<a id="gyro_fft_usage"></a>
### Usage
```
gyro_fft <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## heater
Source: [drivers/heater](https://github.com/PX4/PX4-Autopilot/tree/main/src/drivers/heater)


### Description
Background process running periodically on the LP work queue to regulate IMU temperature at a setpoint.

This task can be started at boot from the startup scripts by setting SENS_EN_THERMAL or via CLI.

<a id="heater_usage"></a>
### Usage
```
heater <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## land_detector
Source: [modules/land_detector](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/land_detector)


### Description
Module to detect the freefall and landed state of the vehicle, and publishing the `vehicle_land_detected` topic.
Each vehicle type (multirotor, fixedwing, vtol, ...) provides its own algorithm, taking into account various
states, such as commanded thrust, arming state and vehicle motion.

### Implementation
Every type is implemented in its own class with a common base class. The base class maintains a state (landed,
maybe_landed, ground_contact). Each possible state is implemented in the derived classes. A hysteresis and a fixed
priority of each internal state determines the actual land_detector state.

#### Multicopter Land Detector
**ground_contact**: thrust setpoint and velocity in z-direction must be below a defined threshold for time
GROUND_CONTACT_TRIGGER_TIME_US. When ground_contact is detected, the position controller turns off the thrust setpoint
in body x and y.

**maybe_landed**: it requires ground_contact together with a tighter thrust setpoint threshold and no velocity in the
horizontal direction. The trigger time is defined by MAYBE_LAND_TRIGGER_TIME. When maybe_landed is detected, the
position controller sets the thrust setpoint to zero.

**landed**: it requires maybe_landed to be true for time LAND_DETECTOR_TRIGGER_TIME_US.

The module runs periodically on the HP work queue.

<a id="land_detector_usage"></a>
### Usage
```
land_detector <command> [arguments...]
 Commands:
   start         Start the background task
     fixedwing|multicopter|vtol|rover|airship Select vehicle type

   stop

   status        print status info
```
## load_mon
Source: [modules/load_mon](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/load_mon)


### Description
Background process running periodically on the low priority work queue to calculate the CPU load and RAM
usage and publish the `cpuload` topic.

On NuttX it also checks the stack usage of each process and if it falls below 300 bytes, a warning is output,
which will also appear in the log file.

<a id="load_mon_usage"></a>
### Usage
```
load_mon <command> [arguments...]
 Commands:
   start         Start the background task

   stop

   status        print status info
```
## logger
Source: [modules/logger](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/logger)


### Description
System logger which logs a configurable set of uORB topics and system printf messages
(`PX4_WARN` and `PX4_ERR`) to ULog files. These can be used for system and flight performance evaluation,
tuning, replay and crash analysis.

It supports 2 backends:
- Files: write ULog files to the file system (SD card)
- MAVLink: stream ULog data via MAVLink to a client (the client must support this)

Both backends can be enabled and used at the same time.

The file backend supports 2 types of log files: full (the normal log) and a mission
log. The mission log is a reduced ulog file and can be used for example for geotagging or
vehicle management. It can be enabled and configured via SDLOG_MISSION parameter.
The normal log is always a superset of the mission log.

### Implementation
The implementation uses two threads:
- The main thread, running at a fixed rate (or polling on a topic if started with -p) and checking for
  data updates
- The writer thread, writing data to the file

In between there is a write buffer with configurable size (and another fixed-size buffer for
the mission log). It should be large to avoid dropouts.

### Examples
Typical usage to start logging immediately:
```
logger start -e -t
```

Or if already running:
```
logger on
```

<a id="logger_usage"></a>
### Usage
```
logger <command> [arguments...]
 Commands:
   start
     [-m <val>]  Backend mode
                 values: file|mavlink|all, default: all
     [-x]        Enable/disable logging via Aux1 RC channel
     [-e]        Enable logging right after start until disarm (otherwise only
                 when armed)
     [-f]        Log until shutdown (implies -e)
     [-t]        Use date/time for naming log directories and files
     [-r <val>]  Log rate in Hz, 0 means unlimited rate
                 default: 280
     [-b <val>]  Log buffer size in KiB
                 default: 12
     [-p <val>]  Poll on a topic instead of running with fixed rate (Log rate
                 and topic intervals are ignored if this is set)
                 values: <topic_name>
     [-c <val>]  Log rate factor (higher is faster)
                 default: 1.0

   on            start logging now, override arming (logger must be running)

   off           stop logging now, override arming (logger must be running)

   stop

   status        print status info
```
## mag_bias_estimator
Source: [modules/mag_bias_estimator](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/mag_bias_estimator)


### Description
Online magnetometer bias estimator.

<a id="mag_bias_estimator_usage"></a>
### Usage
```
mag_bias_estimator <command> [arguments...]
 Commands:
   start         Start the background task

   stop

   status        print status info
```
## manual_control
Source: [modules/manual_control](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/manual_control)


### Description
Module consuming manual_control_inputs publishing one manual_control_setpoint.


<a id="manual_control_usage"></a>
### Usage
```
manual_control <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## microdds_client
Source: [modules/microdds_client](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/microdds_client)


### Description
MicroDDS Client used to communicate uORB topics with an Agent over serial or UDP.

### Examples
```
microdds_client start -t serial -d /dev/ttyS3 -b 921600
microdds_client start -t udp -h 127.0.0.1 -p 15555
```

<a id="microdds_client_usage"></a>
### Usage
```
microdds_client <command> [arguments...]
 Commands:
   start
     [-t <val>]  Transport protocol
                 values: serial|udp, default: udp
     [-d <val>]  serial device
                 values: <file:dev>
     [-b <val>]  Baudrate (can also be p:<param_name>)
                 default: 0
     [-h <val>]  Host IP
                 values: <IP>, default: 127.0.0.1
     [-p <val>]  Remote Port
                 default: 15555
     [-l]        Restrict to localhost (use in combination with
                 ROS_LOCALHOST_ONLY=1)

   stop

   status        print status info
```
## netman
Source: [systemcmds/netman](https://github.com/PX4/PX4-Autopilot/tree/main/src/systemcmds/netman)


  ### Description
  Network configuration manager saves the network settings in non-volatile
  memory. On boot the `update` option will be run. If a network configuration
  does not exist. The default setting will be saved in non-volatile and the
  system rebooted.
  On Subsequent boots, the `update` option will check for the existence of
  `net.cfg` in the root of the SD Card.  It will saves the network settings
  from `net.cfg` in non-volatile memory, delete the file and reboot the system.

  The `save` option will `net.cfg` on the SD Card. Use this to edit the settings.
  The  `show` option will display the network settings  to the console.

  ### Examples
  $ netman save           # Save the parameters to the SD card.
  $ netman show           # display current settings.
  $ netman update -i eth0 # do an update

<a id="netman_usage"></a>
### Usage
```
netman <command> [arguments...]
 Commands:
   show          Display the current persistent network settings to the console.

   update        Check SD card for net.cfg and update network persistent network
                 settings.

   save          Save the current network parameters to the SD card.
     [-i <val>]  Set the interface name
                 default: eth0
```
## pwm_input
Source: [drivers/pwm_input](https://github.com/PX4/PX4-Autopilot/tree/main/src/drivers/pwm_input)


### Description
Measures the PWM input on AUX5 (or MAIN5) via a timer capture ISR and publishes via the uORB 'pwm_input` message.


<a id="pwm_input_usage"></a>
### Usage
```
pwm_input <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## rc_update
Source: [modules/rc_update](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/rc_update)


### Description
The rc_update module handles RC channel mapping: read the raw input channels (`input_rc`),
then apply the calibration, map the RC channels to the configured channels & mode switches
and then publish as `rc_channels` and `manual_control_input`.

### Implementation
To reduce control latency, the module is scheduled on input_rc publications.


<a id="rc_update_usage"></a>
### Usage
```
rc_update <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## replay
Source: [modules/replay](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/replay)


### Description
This module is used to replay ULog files.

There are 2 environment variables used for configuration: `replay`, which must be set to an ULog file name - it's
the log file to be replayed. The second is the mode, specified via `replay_mode`:
- `replay_mode=ekf2`: specific EKF2 replay mode. It can only be used with the ekf2 module, but allows the replay
  to run as fast as possible.
- Generic otherwise: this can be used to replay any module(s), but the replay will be done with the same speed as the
  log was recorded.

The module is typically used together with uORB publisher rules, to specify which messages should be replayed.
The replay module will just publish all messages that are found in the log. It also applies the parameters from
the log.

The replay procedure is documented on the [System-wide Replay](https://docs.px4.io/main/en/debug/system_wide_replay.html)
page.

<a id="replay_usage"></a>
### Usage
```
replay <command> [arguments...]
 Commands:
   start         Start replay, using log file from ENV variable 'replay'

   trystart      Same as 'start', but silently exit if no log file given

   tryapplyparams Try to apply the parameters from the log file

   stop

   status        print status info
```
## send_event
Source: [modules/events](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/events)


### Description
Background process running periodically on the LP work queue to perform housekeeping tasks.
It is currently only responsible for tone alarm on RC Loss.

The tasks can be started via CLI or uORB topics (vehicle_command from MAVLink, etc.).

<a id="send_event_usage"></a>
### Usage
```
send_event <command> [arguments...]
 Commands:
   start         Start the background task

   stop

   status        print status info
```
## sensor_baro_sim
Source: [modules/simulation/sensor_baro_sim](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/simulation/sensor_baro_sim)


### Description



<a id="sensor_baro_sim_usage"></a>
### Usage
```
sensor_baro_sim <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## sensor_gps_sim
Source: [modules/simulation/sensor_gps_sim](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/simulation/sensor_gps_sim)


### Description



<a id="sensor_gps_sim_usage"></a>
### Usage
```
sensor_gps_sim <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## sensor_mag_sim
Source: [modules/simulation/sensor_mag_sim](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/simulation/sensor_mag_sim)


### Description



<a id="sensor_mag_sim_usage"></a>
### Usage
```
sensor_mag_sim <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## sensors
Source: [modules/sensors](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/sensors)


### Description
The sensors module is central to the whole system. It takes low-level output from drivers, turns
it into a more usable form, and publishes it for the rest of the system.

The provided functionality includes:
- Read the output from the sensor drivers (`sensor_gyro`, etc.).
  If there are multiple of the same type, do voting and failover handling.
  Then apply the board rotation and temperature calibration (if enabled). And finally publish the data; one of the
  topics is `sensor_combined`, used by many parts of the system.
- Make sure the sensor drivers get the updated calibration parameters (scale & offset) when the parameters change or
  on startup. The sensor drivers use the ioctl interface for parameter updates. For this to work properly, the
  sensor drivers must already be running when `sensors` is started.
- Do sensor consistency checks and publish the `sensors_status_imu` topic.

### Implementation
It runs in its own thread and polls on the currently selected gyro topic.


<a id="sensors_usage"></a>
### Usage
```
sensors <command> [arguments...]
 Commands:
   start
     [-h]        Start in HIL mode

   stop

   status        print status info
```
## tattu_can
Source: [drivers/tattu_can](https://github.com/PX4/PX4-Autopilot/tree/main/src/drivers/tattu_can)


### Description
Driver for reading data from the Tattu 12S 16000mAh smart battery.


<a id="tattu_can_usage"></a>
### Usage
```
tattu_can <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
## temperature_compensation
Source: [modules/temperature_compensation](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules/temperature_compensation)


### Description
The temperature compensation module allows all of the gyro(s), accel(s), and baro(s) in the system to be temperature
compensated. The module monitors the data coming from the sensors and updates the associated sensor_correction topic
whenever a change in temperature is detected. The module can also be configured to perform the coeffecient calculation
routine at next boot, which allows the thermal calibration coeffecients to be calculated while the vehicle undergoes
a temperature cycle.


<a id="temperature_compensation_usage"></a>
### Usage
```
temperature_compensation <command> [arguments...]
 Commands:
   start         Start the module, which monitors the sensors and updates the
                 sensor_correction topic

   calibrate     Run temperature calibration process
     [-g]        calibrate the gyro
     [-a]        calibrate the accel
     [-b]        calibrate the baro (if none of these is given, all will be
                 calibrated)

   stop

   status        print status info
```
## tune_control
Source: [systemcmds/tune_control](https://github.com/PX4/PX4-Autopilot/tree/main/src/systemcmds/tune_control)


### Description

Command-line tool to control & test the (external) tunes.

Tunes are used to provide audible notification and warnings (e.g. when the system arms, gets position lock, etc.).
The tool requires that a driver is running that can handle the tune_control uorb topic.

Information about the tune format and predefined system tunes can be found here:
https://github.com/PX4/PX4-Autopilot/blob/main/src/lib/tunes/tune_definition.desc

### Examples

Play system tune #2:
```
tune_control play -t 2
```

<a id="tune_control_usage"></a>
### Usage
```
tune_control <command> [arguments...]
 Commands:
   play          Play system tune or single note.
     error       Play error tune
     [-t <val>]  Play predefined system tune
                 default: 1
     [-f <val>]  Frequency of note in Hz (0-22kHz)
     [-d <val>]  Duration of note in us
     [-s <val>]  Volume level (loudness) of the note (0-100)
                 default: 40
     [-m <val>]  Melody in string form
                 values: <string> - e.g. "MFT200e8a8a"

   libtest       Test library

   stop          Stop playback (use for repeated tunes)
```
## work_queue
Source: [systemcmds/work_queue](https://github.com/PX4/PX4-Autopilot/tree/main/src/systemcmds/work_queue)


### Description

Command-line tool to show work queue status.


<a id="work_queue_usage"></a>
### Usage
```
work_queue <command> [arguments...]
 Commands:
   start

   stop

   status        print status info
```
