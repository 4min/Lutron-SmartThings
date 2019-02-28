# RadioRA Classic Smart Bridge

REST server for controlling Lutron RadioRA Classic light switches and dimmers (this original Lutron lighting system is also called RadioRA 1 or Legacy RadioRA). The RadioRA Classic Smart Bridge must by run on a system (such as a Raspberry Pi) with a physical RS232 serial connection to one of Lutron's RadioRA Classic hardware serial modules such as the RA-RS232 or Chronos RA-SBT-CHR.

Credit goes to Stephen Harris at Homemations for developing this Python-based Lutron RadioRA Classic server.

## Required Hardware

* Lutron hardware module that supports RS-232 communication with a RadioRA Classic system:
    - [RadioRA RS232 Serial Interface](http://www.lutron.com/TechnicalDocumentLibrary/044005c.pdf) (RA-RS232)
    - [RadioRA Chronos System Bridge](http://www.lutron.com/TechnicalDocumentLibrary/044037b.pdf) (RA-SBT-CHR)

* Raspberry Pi or other server to run the Bridge

* RS232 serial cable (or direct wire to Raspberry Pi GPIO pins using a MAX3232 RS-232 male adapter)

## Configuration

1. Using your Lutron RadioRA Classic RS232 module, you must physically assign each Zone to a specific switch or dimmer. See the manual for your Lutron hardware module for these steps. Any Zones that are not configured on the hardware device will show up as Unassigned when querying zones later.

2. Connect the RS-232 module to the host that will be running the RadioRA Classic Smart Bridge with a serial cable.

3. Configure SERIAL_TTY environment variable to point to the /dev/tty device connected to the RS-232 physical hardware.

4. Start the RadioRA Classic Smart Bridge (execute ./run.sh in simple case)

5. Use your browser to go to http://<yourhosthere>:8333/api/

NOTE: A zone is any individual RadioRA Classic dimmer, switch, GRAFIK Eye Interface, or Sivoia Control. Each RadioRA Classic system has a maximum of 32 zones. Multiple instances of the Bridge can be run on different ports with serial cables connected to different RadioRA hardware modules to support an unlimited number of zones 

### NOTES

* the RadioRA Classic serial APIs provided by Lutron have no way to read the current dimmer level, only on/off state
* does not support native 15 Phantom Buttons or Room/Scene features of the Lutron hardware modules
* does not support fade time for dimmer state changes
* does not support setting LED lights on other master controls
* there is no security for the port, anyone with access can control the lights

### FUTURE

- support Phantom Buttons to control groups of zones (faster setting entire groups than individual one-by-one zone turning on/off) ... or use one of the Phantom Buttons to specify which lights should flash for alarms

- SGS command support for Set GRAFIK Eye Scene (applies to GRAFIK Eye Interfaces and Sivoia Controls)

- remember the previous dimmer level settings, use as defaults for on if not configured

- support for asynchronous ZMP zone monitoring (with ZMPMON / ZMPMOFF support) ... Constantly monitor RS232 for status updates...note in manual: "If an external RS232 system is not setup to continuously monitor the RS232 port, asking the RadioRA System for status is a useful way to monitor RadioRA System actions or get an updated status. However, using the Serial Device in this manner will cause you to miss MBP commands (Master Control Button Press) and LZC commands (Local Zone Change)."