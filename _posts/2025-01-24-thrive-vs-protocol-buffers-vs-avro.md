When systems exchange data there are some obvious choices like csv, JSON or XML. The advantage of these are the human readability and wide adoption amongst different languages and frameworks. 

However, they have some a drawbacks. First, no schema is enforced which means it is up to the reader of the data how to interpret the data. Second, as data is not encoded, file sizes are typically large for these formats, consuming not only space but also have slower I/O as an effect. In response to that multiple binary encoding formats have been developed like BSON or BJSON. The flaw with these kinds of encoding is that they preserve the JSON-like encoding, i.e. the field names are encoded too so the resulting objects do not save much more space than their un-encoded originals. 

As a response binary encoding libraries have been developed. 

One advantage of Thrift and Protocol Buffers is that they don't encode the field names, rather they reference the fields using tags. These tags are numbers, starting with 1. In contrast, Avro stores the field names, making it a little less compact than the other two. 

These formats support schema evolution. This means you can add or remove fields from the schema and no matter whether application code has picked up these changes in the schema or not, theer won't be any issues. There's two types of compatibility:
- Forward compatability: Old code can read data written by new code.
- Backward compatability: New code can read data written by old code.

From a use case perspective, Avro is used in big data framworks like Hadoop. Since it is storing the data in a row-oriented fashion it is good for transaction-heavy workloads. Thrift and Protocol Buffers are often seen in the context of microservices, i.e. they are used to couple different services perhaps written in different languages.

Finally, it's worth to say that Protocol Buffers make us of auto-generated code based of a schema definition (the .proto file). This is creating the stubs, which can be used to implement a client and a server in case of gRPC.