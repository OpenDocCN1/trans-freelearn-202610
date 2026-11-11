# XML-RPC

As developers in today's world, we should be familiar with XML. On the surface, it is a group of data that is packaged and organized neatly into opening and closing tags. A deeper look tells us that this structure of XML makes it easy for machines to read and process. Thus, while XML can be hard on human eyes, we know it is designed for machine communication. However, without any sort of agreed structure of the XML document by the machines, the advantages of XML are effectively eliminated. This is where XML-RPC comes in.

RPC is an acronym for **Remote Procedure Call**; developed by **David Winer** of UserLand software in 1996. Its purpose is to allow applications, regardless of how different each program or the purpose of each program, to communicate with each other across a network in a standardized manner.

In computer terms, a procedure call is that which gets executed when the operating system communicates to the input devices about what you are doing: *Which key did you just hit on the keyboard? Where did you move the mouse? What did you just click on, with the mouse?*

XML-RPC carries this idea into the networking world (the "Remote" part of RPC) by creating a standard for one program to get information from another program across the network. Program A sends a remote procedure call to Program B. This call may include parameters that Program B needs to retrieve the data.

For example, if the query is against a list of people's name: *Do you want to retrieve a list of only those whose first name is "Peter"? Do you want to narrow your search down to a city?* The requests, and all its parameters, are formatted in a generic way that Program B understands. Regardless of the data type or size, Program B returns the answer back in a generic way that Program A understands. Program A can then do with the data whatever the user requested.

# XML-RPC Structure

Two programs communicating across a network is obviously very different from an operating system talking with a mouse. An operating system has the advantage of super high speed internal buses and the ability to talk on a lower machine level. A procedure call using XML-RPC must be program neutral and friendly to the network transport protocol. It does not have the luxury of constantly polling the other machine hundreds of times per second. Thus, XML-RPC communication must accomplish its mission in the most efficient means possible in the lowest common denominator. This is accomplished by dividing calls into strictly formed XML requests and responses.