# Mule4-read-transform-largefiles
How to deal large files using Mule4 components, read, transform and write to a working directory

This sample project demonstrate two solutions to consume large files, 

### Solution 1: Enabling streaming using repeatable streams
This convenient streaming option available in Mule 4 allows storing temporary chunks of the stream on disk or in memory (see the Streaming in Mule Apps for more details regarding the strategies), so it can be accessed by multiple processors and optionally at the same time. This solution implements the file store strategy where small temporary files of 2 mb will be stored on disk.

### Solution 2: Enabling streaming using MIME type 
For this option to work there are a few conditions:

- The data producer (file listener in this case) needs to indicate the output is streaming=true in the media type
- The format of files is limited, only JSON, XML, CSV or Excel (XLSX) are supported, this solution indicates application/xml directives in the media type
- Only in the case of XML streaming, DataWeave needs some help to be able to identify the minimum unit to stream, in other words, what are the repeating nodes to be streamed. This can be achieve by defining ‘collectionPath’ as attribute in the media type, in this solution it is set to ‘root’ which is the parent of Entry repeatable elements
- Data elements must be accessed sequentially one after the other, for example when reading the second Entry node of the XML file under test in this article, the first or last Entry node cannot be accessed

To demonstrate above mentioned solutions, two flows were implemented to extract some required information from the message and then write into another file in disk the output stream.
- It starts by reading a file from a local input directory,
- extracts from information from the attributes (e.g. file name)
- then extracts relevant information from the consumed XML file
- finally writes the file payload to a output directory

Is important to mention that the common transformation component defines two important directives:
- the option deferred = true, which indicates that the output of this transformation is an output stream
- the option indent = true, when dealing with large payloads it can reduce the file size dramatically.

### About the file structure

The file is a relatively flat XML file with a few child elements, the file was obtained from the XML data repository and it contains hundreds of thousands of protein descriptions, including function, domain structure and other characterised variants. The XML structure looks like:

`
<?xml version="1.0" encoding="UTF-8"?>
<root>
   <Entry>
      <AC />
      <Mod />
      <Descr />
      <Species />
      <Org />
      <Ref />
       <!-- a lot of more of data elements here -->
   </Entry>
   <Entry>
      <AC />
      <Mod />
      <Descr />
      <Species />
      <Org />
      <Ref />
       <!-- a lot of more of data elements here -->
   </Entry>
</root>
`

## Running the flow

To demonstrate the memory can handle large file the meta space can be setup to less than 1gb, the JVM maximum memory allocation was configured with explicit parameters:

`
-Xms1024m -Xmx1024m
`

Please note that properties are configured based on the environment, by default the environment is set to 'dev' and the environment property can be changed in the global elements.
## Contributing
Pull requests are welcome.
