
# C3DFBS
  <a href="https://contactile.com/"><img src="https://github.com/contactile/c3dfbs/assets/122410361/c9684832-b24c-4592-ae98-df30c9543855" alt="Contactile logo"></a>

<h3 align="center">3D Force Button Sensor C3DFBS Host Library</h3>

## Installation 
### Platform IO
 Include the C3DFBS library in your code by adding a dependency in your *platformio.ini* file.
> lib_deps = https://github.com/contactile/c3dfbs.git @ ^0.3.0 

### Arduino
The easiest way to include the C3DFBS library in an Arduino sketch is to first [download this repository](https://github.com/contactile/c3dfbs/archive/refs/heads/main.zip), then navigate to *Sketch > Include Library > Add .ZIP Library...* and select the zip file.

## Hardware
This host library is suitable for communication with Contactile 3D Force Button Sensors.

<img src="https://github.com/contactile/c3dfbs/assets/122410361/b902b3b0-8bda-4a7a-9cc4-94cfaff80ba6" alt="3D Force Button Sensor">


## Usage
Include the library in your project.
 ```cpp
 #include <C3DFBS.h>
 ```
 Create a class instance. In this case, we're using the 3D Force Button Sensor in SPI mode.
  ```cpp
/* Create a C3DFBS class instance in SPI mode */
const  uint8_t ss_pin = SS; // Slave select pin
const  uint8_t int_pin = 8; // Used for interrupts/signalling
const  uint8_t reset_pin = 9; // Used to resetthe C3DFBS sensor

C3DFBS sensor = C3DFBS(C3DFBS::COMMS_SPI, ss_pin, int_pin , reset_pin);
  ```
Initialise the sensor by calling begin(). To 'zero' the force readings, removeBias() must be called at least once when no forces are being applied to the sensor. Configure which data you'd like to read from the sensor by calling setDataFields(...), then finally begin data streaming by calling startDataStream().
 ```cpp
 void setup()
 {
  	sensor.begin();
  	
  	// Call when no forces are applied to the sensor
 	sensor.removeBias();
 	
 	// Request data
 	uint32_t fields = C3DFBS::AllForce | C3DFBS::Temperature;
 	sensor.setDataFields(fields);
 	
 	// Start data acquisition
 	sensor.startDataStream();
	 
 	// The rest of your setup code... 
 }
 ```
 Read data from the sensor by calling readDataStream(...), which takes a pointer to a DataPacket struct.
 
 ```cpp
 void loop()
 {
	 C3DFBS::DataPacket data;
	 
	 // Read data
	 auto status = sensor.readDataStream(&data);
	 
	 if(status == C3DFBS::SUCCESS)
	 {
       // Print data, or do something else with the data
       sensor.printData(&data,true);
	 }
}
 ```
 
**For more detailed usage, see the example Arduino sketch located at *\examples\Streaming\Streaming.ino*.**



## License
The C3DFBS host library is licensed under the [GNU General Public License v3.0](https://github.com/contactile/c3dfbs/blob/main/LICENSE).
