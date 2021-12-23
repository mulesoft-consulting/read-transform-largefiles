# Mule4-read-transform-largefiles
How to deal large files using Mule4 components, read, transform and write to a working directory

This flow demonstrates how a file connector works with repeatable streams to read a large file from a local working directory. 
- It starts by reading a file from a local input directory,
- Sets a set of variables related to the file name, destination and correlation id,
- then converts the file from csv to json, here the transformation is set to deferred = true
- finally writes the file payload to a output directory

## Running the flow

To demonstrate the memory can handle large file the meta space can be setup to less than 1gb, for example 

`
-XX:MaxMetaspaceSize=512m
`

Please note that properties are configured based on the environment, by default the environment is set to 'dev' and the environment property can be changed in the global elements.
## Contributing
Pull requests are welcome.
