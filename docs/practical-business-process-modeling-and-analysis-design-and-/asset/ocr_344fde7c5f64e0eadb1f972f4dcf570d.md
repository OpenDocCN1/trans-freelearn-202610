The first version of the standard was published by the **Business Process Management Initiative**
(BPMI) in 2004 as Business Process Modeling Notation. Since 2005, when BPMI merged with OMG,
an organization known for developing and maintaining many technology standards, BPMN has been
developed by OMG. As part of this work, version 1.1 of the standard was published in 2008, followed
by version 1.2 in 2009.

However, the most important moment in the development of BPMN was in 2011, when version 2.0 of
the specification was published, which, with minor modifications (the latest version of the standard
is 2.0.2, published in 2014 as the ISO/IEC 19510 standard), is still in force today.

Version 2.0 of the BPMN standard brought a lot of significant changes; many new object types and
icons were added and some of the elements introduced in the earlier version of the specification were
changed. So, in this book, we will be dealing with the current version.

The first important thing to know about version 2.0 is that the BPMN acronym now stands for **Business**
**Process Model and Notation**. This name change naturally has a deeper rationale, which we'll address
in a moment, but for those uninterested in the technical details, it has brought one very important
benefit: if, in the course of searching for answers to questions related to BPMN, you find an article
in which the author uses a different acronym expansion than Business Process Model and Notation,
it's quite likely outdated content that doesn't take into account the latest version of the specification.

Another extremely important change introduced by version 2.0 of the standard was the definition
of how data is exchanged between different tools supporting BPMN. This is very important as well.
While earlier versions of the standard allowed generating – for some types of process diagrams – code
to automate processes in **Business Process Execution Language (BPEL)**, there was no reliable way
to go from BPEL code to a BPMN diagram that would work for every process diagram. For a while,
another technology standard served as a workaround for this problem: **XML Process Definition**
**Language (XPDL)**, developed by the – unfortunately no longer functional – **Workflow Management**
**Coalition (WfMC)**. However, this was not an optimal situation, because not all elements possible to
describe in WfMC could be exchanged between different tools using XPDL.

Version 2.0 of the standard introduced a special data exchange format, using the XML file format and
described in the Diagram Interchange section (_chapter 12.1_) of the standard, which is often referred
to as BPMN DI or BPMN XML. This ensures that any business process diagram can be saved as an
XML file for data exchange with other modeling or process automation tools.

In addition, to take care of efficient data exchange, a special **BPMN Model Interchange Working**
**Group (BPMN MIWG)** was established – for more than 10 years, it has provided a forum for tool
developers to collaborate, conducted annual diagram exchange demonstrations, and maintained and
developed a library of test models through which it is possible to verify the level of support of BPMN
notation by a given tool. Test results are presented in the form of a table, the current state of which,
at the time of writing this book, is shown in the following screenshot.