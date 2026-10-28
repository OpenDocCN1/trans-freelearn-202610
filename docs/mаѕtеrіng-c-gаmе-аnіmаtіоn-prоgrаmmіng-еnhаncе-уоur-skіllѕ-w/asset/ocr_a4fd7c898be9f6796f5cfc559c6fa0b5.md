```
node->exportData().value();
    ...
}
```

By using std::optional in the return value for exportData(), we can easily skip the entire section in the YAML file if the node has no state to save. Without the optional keyword, we would need an additional check to determine if the node state needs to be saved or not.

For the loading part of the new behavior data, we must add a new convert template in the file YamlParserTypes.h:

```
template<>
struct convert<ExtendedBehaviorData> {
    static Node encode(const ExtendedBehaviorData& rhs) {
        ...
    }
    static bool decode(const Node& node,
        ExtendedBehaviorData& rhs) {
        ...
    }
}
```

We are using the extended version of the BehaviorData struct since we only store the type of a node, and the YAML parser is the wrong place to create new nodes:

```
struct PerNodeImportData {
    int nodeId;
    graphNodeType nodeType;
    std::map<std::string, std::string> nodeProperties{};
};
struct ExtendedBehaviorData : BehaviorData {
    std::vector<PerNodeImportData> nodeImportData;
};
```

Node recreation will happen in the renderer class when loading a saved file, like the creation of models, instances, and cameras.