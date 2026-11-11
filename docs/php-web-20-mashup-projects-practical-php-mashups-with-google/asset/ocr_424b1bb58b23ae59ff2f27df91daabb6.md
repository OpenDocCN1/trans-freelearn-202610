# The Incident Class

This class is located inside the classes directory of the example code. This is the only model class we need.

```
<?php
    class Incident {
        private $incidentType;
        private $address;
        private $area;
        public function getIncidentType() { return $this->incidentType; }
        public function getAddress() { return $this->address; }
        public function getArea() { return $this->area; }
        public function setIncidentType($t) {$this->incidentType = $t; }
        public function setAddress($a) { $this->address = $a; }
        public function setArea($a) { $this->area = $a; }
    }
?>
```

Besides the parser class we will create later, this is the only class that truly knows anything about the CHIP site. Even then, its knowledge is limited to the structure of incident data. All this class will do is hold information about an incident, and provide getter and setter functions for the information. We're going to keep the incident type, address, and general area. In the interest of saving space, our first application will not retrieve the time of the incident.

# The DOM Parser Class

The event-based SAX Parsers from Chapter 1 defined parsing functions for child objects to implement, set up the parser, and had a generic function that executed the parsing of the documents. Our new parent DOM parser class, named DomParser with a file name of clsDomParser.php in the examples code, will function in a similar fashion. It will hold generic functions that all parsers must execute.

In the PHP DOM case, the generic tasks for this class will be to create the DOM and decide whether to load the data as HTML or XML. The decision on what to use is not made here. That will be tied to the implementation class. All this class will do is call the appropriate PHP functions, depending on what the executing class wants.

```
<?php
    abstract class DomParser {
        protected $dom;
```