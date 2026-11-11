The purpose of RDF is to describe a web resource, that is, to describe something on the Internet. For example, if a shopping website lists the price of something, what exactly is a price? Is it in American Dollars? Mexican Pesos? Russian Rubles? For a website, what exactly is a timestamp? Should a machine parser treat a timestamp in 12-hour notation different from a timestamp in 24-hour notation? XML, at a very high level, was supposed to allow groups to standardize on a transaction format. Implementation details were left to the parties of interests because XML is just a language. RDF is the next evolution of that original goal. It gives us a framework for that implementation. By defining what a timestamp is, any machine or human that encounters that RDF document will know, without any ambiguity, what that timestamp is, what it means, and what format it should be in.

The basic concepts and syntax of RDF is fairly simple and straightforward. RDF groups things in what it calls triples. A triple basically says, "A something has a property whose value is something". Triples use the grammar concepts of subject, predicate, and object. In the sentence, "The page has a size of 21 kilobytes", the page is the subject. The predicate is the property, in this case, size. The object is the value of that property, 21 kilobytes. Typically in RDF, the subject is represented by an about attribute of a parent element. The property and value are represented by element and value pairs under that parent element. The page size sentence could be represented as follows in XML notation:

```
<rdf:RDF
    Xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    Xmlns="http://www.example.org/pageProperties"
>
<rdfDescription rdf:about="http://www.shuchow.com/thecats.html">
    <creator>Shu Chow</creator>
    <title>My Cats</title>
    <lastMod>01/24/22007</lastMod>
    <size>21 kilobytes</size>
</rdf:Description>
</rdf:RDF>
```

In RDF, every element must be namespaced. The rdf namespace is required, and must point to http://www.w3.org/1999/02/22-rdf-syntax-ns. This gives us access to the core RDF elements that structure this document as an RDF document. In this short document, we access the RDF elements three times — once as the root element of the document, once more to identify a resource using the Description element, and once more to identify the specific resource with the about attribute. In human language form, the title can be stated as, "A web resource at http://www.shuchow.com/thecats.html, has a title property, whose value is 'My Cats'". Even more casually, we can say, "The page's title is 'My Cats'".