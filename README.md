**Usb Learnings**

1.What is USB?

**2. Architectural Overview**

**2.1 USB System Description**

• USB interconnect
• USB devices
• USB host

**USB interconnect: **
   is the manner in which USB devices are connected to and communicate with the host, it includes following

• Bus Topology:  Connection model between USB devices and the host
• Inter-layer relationship: Protocol stack (Physical, link, transaction and device/SW layer) 
• Data Flow model: data movement between producers(Host) and consumers(functional devices) using different trasfer types (Control, Bulk, INterrupt, Isochronous)
• USB Shedule: Because the bus is shared, HC decides sheduling 

**USB Devices**
Divided in device-classes
• hub
• HID
• Mass Stroage etc..

USB devices are required to carry information for self identification and generic configuration

Device Characterstics:
• USB devices are accessed by a USB address that is assigned when device is attached and enumerated, later we need to cover Endpoints and PIPE.

• Each USB device additionally supports one or more pipes through which the host may communicate with the device.

• All USB device must support a specially designated PIPE at Endpoint Zero to which USB device’s USB control pipe will be attached. (irrespective of functionality it provide).

• Control PIPE at EP0 describe the USB device, following are informations

     • Standard: vendor identification, device class, and power management capability, Device, configuration,interface, and endpoint descriptions carry configuration-related information about the device
     
     • Class: The definition of this information varies, depending on the device class of the USB device.
     
     • USB Vendor: The vendor of the USB device is free to put any information desired here. The format, however, is not determined by this specification. 

![image alt](https://github.com/SDRG1998/USB-Learnings/blob/561ec4acf7b59759d5da3bde6739814c7c76f808/image/Screenshot%202025-12-12%20193559.png)
