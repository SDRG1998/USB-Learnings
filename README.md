**Usb Learnings**

Pending Topics to be covered

Physical Layer, Link Layer, Protocol Layer

1.What is USB?

**Endpoint**

    • An endpoint is a unidirectional data channel inside the USB device.

    • It is implemented as a buffer plus control logic and is identified by:

        • Endpoint number (0–N)

        • Direction (IN = device→host, OUT = host→device)

        • Type (control, bulk, interrupt, isochronous)

     • Endpoints exist only in the device hardware/firmware, , and are described to the host in the USB descriptors

     • Each endpoint is a source or sink for data but cannot start transfers on its own; it only responds when the host talks to it.

**Pipe**

    A pipe is the host-side logical connection to an endpoint

    When the host enumerates a device and reads its descriptors, it creates a pipe for each endpoint it wants to use

    The pipe stores:

        Which device and endpoint address it targets

        Direction and transfer type

        Max packet size, interval, and other parameters

        Queues of pending transfers and completion statu

 endpoint = device’s logical channel, pipe = host’s handle for talking to that channel.

![image alt](https://github.com/SDRG1998/USB-Learnings/blob/1ff27dbf9f712d45b5fad7ec4fe4d6212ba3549f/image/EP_Pipe.png)        
    
**2. Architectural Overview**

![image alt](https://github.com/SDRG1998/USB-Learnings/blob/561ec4acf7b59759d5da3bde6739814c7c76f808/image/Screenshot%202025-12-12%20193559.png)


**2.1 USB System Description**

    • USB interconnect

    • USB devices

    • USB host

**USB interconnect: **

   is the manner in which USB devices are connected to and communicate with the host, it includes following

    • Bus Topology:  Connection model between USB devices and the host, USB physical interconnect is a tiered star topology

    • Inter-layer relationship: Protocol stack (Physical, link, transaction and device/SW layer) 

    • Data Flow model: data movement between producers(Host) and consumers(functional devices) using different trasfer types (Control, Bulk, INterrupt, Isochronous)

    • USB Shedule: Because the bus is shared, HC decides sheduling 

**USB Host**

    • The USB interface to the host computer system is referred to as the Host Controller

    • A root hub is integrated within the host system to provide one or more attachment points.
    
**USB Devices**

Divided in device-classes

    • hub

    • HID

    • Mass Stroage etc..

USB devices are required to carry information for self identification and generic configuration

**Device Characterstics:**

    • USB devices are accessed by a USB address that is assigned when device is attached and enumerated, later we need to cover Endpoints and PIPE.

    • Each USB device additionally supports one or more pipes through which the host may communicate with the device.

    • All USB device must support a specially designated PIPE at Endpoint Zero to which USB device’s USB control pipe will be attached. (irrespective of functionality it provide).

    • Control PIPE at EP0 describe the USB device, following are informations

         • Standard: vendor identification, device class, and power management capability, Device, configuration,interface, and endpoint descriptions carry configuration-related information about the device
     
         • Class: The definition of this information varies, depending on the device class of the USB device.
     
         • USB Vendor: The vendor of the USB device is free to put any information desired here. The format, however, is not determined by this specification. 

**Device Descriptions:**

Two major divisions of device classes exist

    • hubs - hubs have the ability to provide additional USB attachment points
    
    • Functions - 
        A function is a USB device that is able to transmit or receive data or control information over the bus.

        Function is seperate Pheripheral device with a cable that plugs into a port on hub. (HID, Mass-Storage etc..) 

**Bus Protocol**

USB 2.0 - polled bus. The Host Controller initiates all data transfers.

USB 3.0 - asynchronous notifications.

**Attachment of USB Devices**

    • Bus enumeration happens -> Detecting and identifying USB devices, unique add. to device attcahed

    • All USB devices attach to the USB through ports on specialized USB devices known as hubs    

    • Hubs have status bits that are used to report the attachment or removal of a USB device on one of its ports

    • In the case of an attachment, the host enables the port and addresses the USB device through the device’s control pipe at the default address

    • host assigns a unique USB address, determines if the newly attached USB device is a hub or a function

**Removal of USB Devices**

    • When a USB device has been removed the hub disables the port and provides an indication of device removal to the host.

    • If the removed USB device is a hub, system SW removes all the devices attached to the hub.

**Data Flow Types**

    ** • Control Transfers:**

        • Control data is used by the USB System Software to configure devices when they are first attached

            • When a new USB device is plugged in, the host’s USB stack uses control transfers on Endpoint 0, read all descriptors, assign an address, select a configuration, set interface/alternate settings, and set or clear features

        • Other driver software can choose to use control transfers in implementation-specific ways

            • After configuration, class or vendor drivers can still send their own standard, class‑specific, or vendor‑specific control requests 
            
         • Data delivery is lossless.

             • For control transfers, the USB protocol guarantees reliable delivery: packets have CRCs, errors are retried, and the transfer must complete with a clear success or failure status.
        
    **• Bulk Transfers**

        • Bulk data typically consists of larger amounts of data, such as that used for printers, scanners, mass-storage.

        • Bulk data is sequential

        • Reliable exchange of data is ensured at the hardware level by using error detection in hardware and invoking a limited number of retries in hardware. 
        
    **• Interrupt Transfers**

        • Limited-latency transfer to or from a device

            • When the device’s interrupt endpoint is configured, it specifies an interval (polling period)
        
        • Interrupt data typically consists of event notification, characters, or coordinates that are organized as one or more bytes. (eg keyboard, mouse, HID)

    **• Isochronous Transfers**

        • Isochronous transfers are for continuous, real‑time streams (audio, video) where timing is more important than perfect reliability.
        
    
**USB Host: Hardware and Software**

•  The USB host interacts with USB devices through the Host Controller, host is reponsible for following

    • Detecting the attachment and removal of USB devices.

    • Managing control flow between the host and USB devices

    • Managing data flow between the host and USB devices

    • Providing power to attached USB devices

•  The USB System SW (HC Driver/ USB Core driver)on host manages interactions between USB devices and host-based device software (USB class drivers sits on top of USB Host stack)

    • Device enumeration and configuration

    • Isochronous data transfers - Time critical streams (audio/video).

    • Asynchronous data transfers - (control, bulk, interrupt).

    • Power management (Suspend/resume).
    
    • Device and bus management information.
    
**3. USB Data Flow Model**

The USB provides communication services between a host and attached USB devices.
    
However, the simple view an end user sees of attaching one or more USB devices to a host.

Underneath it has multi-layer as following

    • USB Physical Device: A piece of hardware on the end of a USB cable that performs some useful end user function.

    • Client Software: (USB Class drivers) that executes on the host, corresponding to a USB device.

    • USB System Software: USB core (drivers/usb/core) Software that supports the USB in a particular operating system.

    • USB Host Controller (Host Side Bus Interface): The hardware and software that allows USB devices to be attached to a host.

