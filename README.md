# UnifiedRocketControllerLibrary
A library designed from the bottom up to facilitate in the development of flight controller software for High Powered Rockets.

##Features

####Logging
* Logging is set up in a hierarchical fashion. Think JSON. There can be "objects" that are given attributes. 
  * For example, we can have a "Rocket" object. This object can be given the attribute "Pitch". The "Rocket" object can also be the parent of another object. For example, "Pitot Tube". This "Pitot Tube" object can be given attributes "Pressure" and "Speed". In this way, every piece of information about the rocket can be logged and found extremely easily. 
  * This brings in concerns about time based data. Clearly JSON and XML are terrible for this type of data. So, we use CSV files with headers that trace the parent->child relationships. For example, with the pitot tube example above, we would have "Rocket::Pitot Tube::Pressure". This should also let the CSV file's columns be alphabetized in order to clump together related information.  
* In an attempt to make it easier to output and structure the data wanted in the output logging, be it radio or SD card or something else entirely, the library is setup to make it easy to setup the hierarchy of data, then assign a *reference* to the data that should be output. This makes it easier on the user to not have to continuously update the data being sent to the logging devices. The user will only have to update data that the rocket doesn't have any internal concept of, an example of this would be a new type of sensor that doesn't directly record data relating to the flight of the rocket. 
* In order to differentiate between output devices, a Logger object is created, this object is passed a LoggerDevice pointer. This whole Logger object is then passed into the main library which will manage the loggers and output on them on a regular basis. The LoggerDevice class is a virtual class that ensures output devices support the functions required by the Logger class. Data input and output devices are discussed in the next chapter.   
  


####Data I/O devices
* There exist different types of input and output devices. 
    * Stream: A stream of bytes such as an Xbee radio module or a serial communication device. URCL supports:
        * Setting a custom input and output buffer maximum size. 
        * Growable and shrinkable buffer sizes to save on available memory. 
        * Formatted output for easy to use menu systems and formatted data. 
        * Logging information can be output to a stream.
    * Switches: A discrete on/off input signal. 
        * Read the input on/off signal. 
        * Maintains current state of signal.
        * Supports toggling. (EX: Push button)
        * Switch values are loggable by the Logger class. 
    * Discrete output: A discrete on/off output signal. Such as a light. 
        * Set the input/output value. 
        * Output value is available for logging. 
    * Analog input:
    * Analog output:
    * Storage Device: A device that is capable of saving  
        * 
    

####Controller Algorithms

* URCL supports common closed loop controller algorithms. 
    * **PID** - PID controllers are an extremely common control algorithm. URCL supports them and a lot of the caveats that come with using PID controllers. URCL supports: 
        * Any combination of P, I, and D.
        * Anti-Windup using max and min possible values.
        * Applying new setpoint limits on the fly. 
        * Applying new setpoints on the fly. 
    * **BANG-BANG** - You may determine bang bang accuracy is all the accuracy you need. In order to use a bang-bang controller, Simply set up a loggable variable to watch for go over, under, between, or outside certain values. Once this state happens, the bang-bang controller will give an active output for you to apply to whatever it is you want to control. The "loggable variable" part means any of the variables that you can setup to be output to a logging device automatically. This is almost any variable URCL has an internal representation of. You can also feed in any variable you define in your program for more complicated control schemes. 
* Controller algorithms can be fed errors externally in order to support any process that needs to be controlled. The user simply feeds in the current error of the process when it is calculated, and the controller will keep track of current states and output. 
    
* URCL supports open loop control as well. 
    * **Timer** - Setting up a timer from scratch, while trivial, can be a little annoying. The timer URCL contains supports:
        * Setting different times with milliseconds and nanoseconds.
        * Handling the time value rolling over after overflowing. 
        * One-shot or repeating.
        * Activating and deactivating the timer. 
        * Multiple timer instances.

####Sensor Fusion
* URCL supports common sensor fusion techniques in order to put together a more accurate state of the rocket. 
* URCL can take in all sorts of information about your rocket in order predict with. Including:
    * Mass
    * Drag Coefficient
    * Frontal reference area
* Along with these data, URCL has internal variables including: 
    * Orientation (Roll, Pitch, Yaw)
    * Airspeed (Pitot tube)
    * Air pressure (Altimeter)
    * Acceleration
