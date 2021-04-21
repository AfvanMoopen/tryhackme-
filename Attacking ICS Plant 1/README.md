# Attacking ICS Plant #1

Learn how to discover and attack ICS plants using modbus protocol (Modicon / Schneider Electric).

[Attacking ICS Plant #1](https://tryhackme.com/room/attackingics1)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Introduction to OT/ICS

Operational Technology (OT) refers to systems used to monitor and control industrial operations. Industrial Control Systems (ICS) includes systems used to monitor and control industrial processes.

If IT operators have dozens of years of experience securing networks, systems and applications, OT operators are pretty new to this topic. Moreover in OT/ICS development availability is always preferred over integrity and confidentiality. In other words ICS softwares are designed to be fast but, often, insecure.

Supervisory Control and Data Acquisition (SCADA) systems are used to control and automate industrial processes. SCADA systems includes:

- **Supervisory computers**: the servers used to manage the process gathering data on the process and communicating with filed devices (PLC/RTU). In smaller deployments HMI is embedded in a single computer, in larger deploy HMI is installed into a dedicated computer.
- **Programmable Logic Controllers (PLC)**: digital computers used mainly for automating industrial processes. They are used to continuously monitor sensors (input) and make decisions controlling devices (output).
- **Remote Terminal Units (RTU)**: nowadays RTUs and PLCs functionalities overlap with each other. RTUs are usually preferred for wider geographical telemetry whereas PLCs are better with local controls.
- **Communication network**: the network connecting all SCADA components (Ethernet, Serial, telephones, radio, cellular...). Network failures do not necessarily impact negatively on the plant process. Both RTU's and PLC's should be designed to operate autonomously, using the last instruction given from the supervisory system.
- **Human Machine Interface (HMI)**: displays a digitalized representation of the plant. Operators can interact with the plant issuing commands using mouse, keyboards or touch screens. **Operators can make supervisory decisions adjusting or overriding the normal plant behaviour**.

In short and simple words:

- industries are managed by sophisticated, mission critical computers (SCADA systems);
- security is not the first priority in OT/ICS;
- operators can manually override the behaviour of the plant via mouse/keyboard/touchscreen, locally or remotely;
- a malicious software can override the behaviour of the plant like HMI does.

For more information see [Guide to Industrial Control Systems (ICS) Security (NIST 800-82)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-82r2.pdf).

No answer needed

`No answer needed`

## Task 2 Introduction to Modbus protocol

Modbus is an industrial protocol developed by Modicon, acquired by Schneider Electric. Modbus is widely used to connect industrial devices; protocol specification is available and royalty-free.

Modbus protocol is a master/slave protocol: the master reads and writes slaves' registers.

Modbus RTU is usually used via RS-485 (serial network): one master is present with one or more slaves. Each slave has an unique 8-bit address.

Modbus data is used to read and write "registers" which are 16-bit long. The most common register is called "holding register" which is readable and writable; registry type "input register" is readable only. The registers "coil" and "discrete input" are 1-bit long: coils are readable and writable, discrete inputs are readable only.

Modbus register types:

- Discrete Input (Status Input): 1bit, RO
- Coil (Discrete Output): 1bit, R/W
- Input Register: 16bit, RO
- Holding Register: 16bit R/W

Most commonly Modbus function:

| Function Code | Register Type                    |
| ------------- | -------------------------------- |
| 1             | Read Coil                        |
| 2             | Read Discrete Input              |
| 3             | Read Holding Registers           |
| 4             | Read Input Registers             |
| 5             | Write Single Coil                |
| 6             | Write Single Holding Register    |
| 15            | Write Multiple Coils             |
| 16            | Write Multiple Holding Registers |

Modbus TCP encapsulates Modbus RTU request and response data packets in a TCP packet transmitted over standard Ethernet networks. The TCP Modbus standard port is 502.

For more information see [Modbus 101 - Introduction to Modbus](https://www.csimn.com/CSI_pages/Modbus101.html).

Before starting, download the attached script package and install the Modbus TCP library for Python with the following command:

`pip3 install pymodbus==1.5.2`

Which is the function used to read holding registers in pymodbus library?

`read_holding_registers`

Which is the function used to write holding registers in pymodbus library?

`write_register`

## Task 3 Discovery

Connect to the plant using the browser (http://MACHINE_IP) and learn how the bottle-filling plant works.

We have three phases:

- Initialization: the plant is starting from the beginning. The roller moves the first bottle under the nozzle.
- Filling: once a bottle is under the nozzle, the nozzle opens and the water flows in the bottle.
- Moving: once the bottle is filled, the roller starts again moving the next empty bottle under the nozzle.

When the phase 3 ends, the plant starts again with phase 2.

From the three phases described above, we can observe:

- Sensors: used to read a state of the plant.
- Actuators: used to alter the state of the plant.

Example: a sensor can detect if a bottle is under the nozzle, while an actuator can open or close the nozzle.

Mind we can press the ESC button to start the plant from the beginning.

VirtuaPlant can be downloaded from [GitHub](https://github.com/jseidl/virtuaplant/network/members).

How many phases can we observe?

`3`

How many sensors can we observe?

`2`

How many actuators can we observe?

`3`

Using the script discovery.py, how many registers can we count?

`16`

After the plant is started and a bottle is loaded, how many registers are continuously changing their values?

`4`

Which is the minimum observed value?

`0`

Which is the maximum observed value?

`1`

Which registry is holding its value?

`16`

Which registries are set to 1 while the nozzle is filling a bottle?

`2 4`

Which registries are set to 1 while the roller is moving the bottles?

`1 3`

Which is the color of the water level sensor?

`red`

Which is the color of the bottle sensor?

`green`

If you observe the plant at the very beginning, which is the registry associated with the roller?

`3`

Based on the previous answer, which is the registry associated with the water level sensor?

`1`

## Task 4 Play to learn

On the previous task, we learned a lot about registries:

- Bottle-filling plant exposes 16 registries, but only 5 are used.
- Registry 16 is associated with the start/stop button: 1 means plant is on, 0 means plant is off.
- Registries 1 and 3 usually have the same value, except in the initialization phase.
- Registries 2 and 4 always have the same value.

We observed three phases, and the associated registries are:

- Initialization: [0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1]
- Filling: once a bottle is under the nozzle, the nozzle opens and the water flows in the bottle.
- Moving: once the bottle is filled, the roller starts again moving the next empty bottle under the nozzle.

We also assumed the plant has two sensors and three actuators. Actuators:

- Start/stop button: registry 16.
- Roller engine: registry 3.
- Nozzle: registry 2 or 4. We need to spend additional time on that.

Sensors:

- Water level: 1 (red sensor)
- Bottle position: 2 or 4 (green sensor). We need to spend additional time on that.

Now play with set_registry.py script and find out which registries are associated with the nozzle.

Which is the registry associated with the nozzle?

`4`

## Task 5 Attack

On the previous tasks, we figured out how registries work:

- Registry 1: associated with the bottle sensor. Value is 1 if the bottle is under the nozzle.
- Registry 2: associated with the water level sensor. Value is 1 if the bottle is filled.
- Registry 3: associated with the roller actuators. Value 1 turns the roller on.
- Registry 4: associated with the nozzle. Value 1 opens the nozzle.
- Registry 16: associated with the plant. Value 1 starts the plant

We can abuse the registries forcing a custom value to the above registries. We can do that in many ways.

Mind that the plant manager will shut down the plant if an abnormal condition is detected.

Shutdown the plant and avoid the plant manager starts it again.

`No answer needed`

Start the plant, open the nozzle while bottles are moving.

`No answer needed`

Start the plant, open the nozzle and stop the rollet.

`No answer needed`

Repeat attack in question 1 abusing sensor registries.

`No answer needed`

Repeat attack in question 2 abusing sensor registries.

`No answer needed`

Repeat attack in question 3 abusing sensor registries.

`No answer needed`
