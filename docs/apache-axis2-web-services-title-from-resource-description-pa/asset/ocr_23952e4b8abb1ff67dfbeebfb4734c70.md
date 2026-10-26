```
</input>
<output>
<soap:body use="literal"/>
</output>
</operation>
<operation name="computeHighestMarks">
<soap:operation soapAction="urn:getHighestMarks"/>
<input>
<soap:body use="literal"/>
</input>
<output>
<soap:body use="literal"/>
</output>
</operation>
</binding>
```

Let us generate the code for this WSDL using the wsimport tool, as shown here:

```
wsimport -s wsdl2java StudentsMarksService.wsdl
```

This will generate the org.apache.jaxws.sample1.StudentMarksPort and org.apache.jaxws.sample1.Student classes. Now we will write a sample web service using those generated classes:

```
package org.apache.jaxws.sample1.service;

import org.apache.jaxws.sample1.StudentMarksPort;
import org.apache.jaxws.sample1.Student;

import javax.jws.WebService;

@WebService(
    serviceName = "StudentMarksService",
    portName = "StudentMarksPort",
    targetNamespace = "http://apache.org/jaxws/sample1",
    endpointInterface = "org.apache.jaxws.sample1.StudentMarksPort",
    wsdlLocation = "StudentMarksService.wsdl"
)
public class StudentMarksImpl implements StudentMarksPort {

    public double computeAverage(Student student) {
        double totalMarks = 0;
```