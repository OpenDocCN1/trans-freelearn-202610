```
return node;
}
```

To save the data from the glm::vec3, we create a new YAML node called node and add the three elements x, y, and z of the glm::vec3 to the node. The node is then returned to the caller of the encode() method.

Reading back the data from a node into a glm::vec3 variable is done with the decode() method:

```
static bool decode(const Node& node, glm::vec3& rhs) {
    if (!node.IsSequence() || node.size() != 3) {
        return false;
    }
```

Checking the node type for the right type and size is optional, but it is a good style assuring we have the correct data for the custom data type to prevent runtime errors. Skipping this check and trying to parse the wrong data type will lead to an exception, terminating the entire program if unhandled.

Then, we read the data from a node by the sequence index, and set the three elements x, y, and z of the glm::vec3 to the float values from the node:

```
rhs.x = node[0].as<float>();
rhs.y = node[1].as<float>();
rhs.z = node[2].as<float>();
return true;
};
```

After defining the encode() and decode() methods, we can exchange data between a YAML node and a glm::vec3 by a normal assignment:

```
glm::vec3 data;
node["rotation"] = data.isWorldRotation;
data.isWorldRotation = node["rotation"].as<glm::vec3>();
```

The same two methods are implemented for the InstanceSettings struct, helping us to read the settings of the instance directly back into a variable of the type InstanceSettings. To avoid polluting the header for our parser class, a new header called YamlParserTypes.h has been created in the tools folder. The YamlParserTypes.h header will be included in the header of the YamlParser class to make the new conversions available.