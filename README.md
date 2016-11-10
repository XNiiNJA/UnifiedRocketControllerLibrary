# UnifiedRocketControllerLibrary
A library designed from the bottom up to facilitate in the development of flight controller software for High Powered Rockets.

##Features

Logging features. 
* Logging is set up in a hierarchical fashion. Think JSON. There can be "objects" that are given attributes. 
  * For example, we can have a "Rocket" object. This object can be given the attribute "Pitch". The "Rocket" object can also be the parent of another object. For example, "Pitot Tube". This "Pitot Tube" object can be given attributes "Pressure" and "Speed". In this way, every piece of information about the rocket can be logged and found extremely easily. 
  * This brings in concerns about time based data. Clearly JSON and XML are terrible for this type of data. So, we use CSV files with headers that trace the parent->child relationships. For example, with the pitot tube example above, we would have "Rocket::Pitot Tube::Pressure". This should also let the CSV file's columns be alphabetized in order to clump together related information.  
* In an attempt to make it easier to output and structure the data wanted in the output logging, be it radio or SD card or something else entirely, the library is setup to make it easy to setup the hierarchy of data, then assign a *reference* to the data that should be output. This makes it easier on the user to not have to continuously update the data being sent to the logging devices. The user will only have to update data that the rocket doesn't have any internal concept of, an example of this would be a new type of sensor that doesn't directly record data relating to the flight of the rocket. 
* In order to differentiate between output devices, a Logger object is created, this object is passed a LoggerDevice pointer. This whole Logger object is then passed into the main library which will manage the loggers and output on them on a regular basis. The LoggerDevice class is a virtual class that ensures output devices support the functions required by the Logger class. Data input and output devices are discussed in the next chapter.   
  


Data input and output devices. 



Controller Algorithms.
* URCL supports common closed loop controller algorithms. 
    * **PID** - PID controllers are an extremely common control algorithm. URCL supports them and a lot of the caveats that come with using PID controllers. URCL supports: 
        * Any combination of P, I, and D.
        * Anti-Windup using max and min possible values.
        * Applying new setpoint limits on the fly. 
        * Applying new setpoints on the fly. 
    * **BANG-BANG** - You may determine bang bang accuracy is all the accuracy you need. In order to use a bang-bang controller, Simply set up a loggable variable to watch for go over, under, between, or outside certain values. Once this state happens, the bang-bang controller will give an active output for you to apply to whatever it is you want to control. You can also feed in any variable you define in your program for more complicated control schemes. 
    
* URCL supports open loop control as well. 
    * **Timer** - Setting up a timer, while trivial, can be a little annoying. The timer URCL contains supports:
        * Setting different times with milliseconds and nanoseconds.
        * Rolling over the timer after overflowing. 
        * One-shot or repeating.
        * Activating and deactivating the timer. 

        

Filter Algorithms.
