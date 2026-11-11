```
abstract public function startElement($parser, $name, $attributes);
abstract public function endElement($parser, $name);
abstract public function characterData($parser, $data);
protected parse($xml) {
    xml_parse($this->parser, $xml);
}
public function __construct($xml) {
    $this->parser = xml_parser_create();
    xml_set_object($this->parser, $this);
    xml_parser_set_option($this->parser, XML_OPTION_CASE_FOLDING, 0);
    xml_set_element_handler($this->parser, "startElement",
        "endElement");
    xml_set_character_data_handler($this->parser, "characterData");
}
}
class PetParser extends SAXParser {
    public $petName;
    public $petId;
    public function startElement($parser, $name, $attributes) {
        if ($name == "Name") {
            $this->tagName = $name;
            $this->petId = $attributes['id'];
        }
    }
    public function characterData($parser, $data) {
        if ($this->tagName == "Name") {
            $this->petName = $data;
        }
    }
    Public function endElement($parser, $name) {
        $this->tagName = "";
    }
}
```

# Examining the Classes

Our abstract class is called SAXParser. Any parser classes we create will implement SAXParser. SAXParser has two properties: tagName and parser. parser is the XML parser that PHP creates with xml_parser_create. By putting it as an object, we can separate out the creation and setup of the parser from the actual XML parsing. We will see that later when we look at the constructor and the parse abstract method. tagName is a container that we use to store and inspect element names. When we are