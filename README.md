# UnifiedRocketryFlightControllerLibrary
A library designed from the bottom up to facilitate in the development of flight controller software for High Powered Rockets.

##Features

Logging features. 
* Logging is set up in a hierarchical fashion. Think JSON. There can be "objects" that are given attributes. 
  * For example, we can have a "Rocket" object. This object can be given the attribute "Pitch". The "Rocket" object can also be the parent of another object. For example, "Pitot Tube". This "Pitot Tube" object can be given attributes "Pressure" and "Speed". In this way, every piece of information about the rocket can be logged and found extremely easily. 
  * This brings in concerns about time based data. Clearly JSON and XML are terrible for this type of data. So, we use CSV files with headers that trace the parent->child relationships. For example, with the pitot tube example above, we would have "Rocket::Pitot Tube::Pressure". This should also let the CSV file's columns be alphabetized in order to clump together related information.  
* In an attempt to make it easier to output and structure the data wanted in the output logging, be it radio or SD card or something else entirely, the library is setup to make it easy to setup the hierarchy of data, then assign a *reference* to the data that should be output. This makes it easier on the user to not have to continuously update the data being sent to the logging devices. The user will only have to update data that the rocket doesn't have any internal concept of, an example of this would be a new type of sensor that doesn't directly record data relating to the flight of the rocket. 
* In order to differentiate between output devices, a Logger object is created, this object is passed a LoggerDevice pointer. This whole Logger object is then passed into the main library which will manage the loggers and output on them on a regular basis. The LoggerDevice class is a virtual class that ensures output devices support the functions required by the Logger class. Data input and output devices are discussed in the next chapter.   
  


Data input and output devices. 
