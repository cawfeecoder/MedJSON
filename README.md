# MedJSON
MedJSON is a format for encoding a variety of medical data structure, including patient records, biometric measurements, and much more.

For example, a typical blood pressure measurement in MedJSON might look like
```json
{
  "type": "Biometric",
  "subtype": "Blood Pressure",
  "data": [{
    "name": "Systolic",
    "measurementType": "mmHg",
    "valueType": "int",
    "value": "100"
  }],
  "metadata": {
    "patient_id": "12345"
  }
```

You may notice that every property on a a MedJSON structure is a string. The motiviation behind this is two-fold. Firstly, 
it's easier to serialize strings into byte[], which allows for efficent transfer, especially in compress based environemnts. 
Secondly, the goal of MedJSON is to be language agnostic. Whether the program utilizing MedJSON is written in Rust, Elixir, Go,
or any other number of languages, MedJSON simply provides information on what the type _should_ be serialized as. There is a case
where languages lack support of specific types, such as JavaScript's limitation on 32-bit integers. Providing a string alleviates
potentially problems of overflows for particular value types.

## MedJSON Proposal

### Section I. Value Types

MedJSON seeks to propose value types which are agnostic between different programming languages. By that, we mean that MedJSON types should be easily mappable. For example, if a measurement is commonly stored as an int64, the value will have a declared as type Int. We expect end users of MedJSON to correctly check for errors and overflows when parsing a type, since their language may not support constructors of the type, such as the JavaScript limitation on 32-bit integers. There are, as always, third-party workarounds such as BigInt.js, which allow the type system presented here to be offered in a single seamless schema. Moreover, we also seek to align our type system with that of GraphQL. While MedJSON can certainly be expressed in a well designed API of any architecture type, we have found that GraphQL is efficent in providing accurute and human readable querying of MedJSON. The align of our type system to GraphQL should allow an ease of use in getting new users to both adopt and get started with MedJSON. Therefore, I propose the following types within MedJSON with equivalent value mapping in popular languages:

| MedJSON Type  | Java Type     | JavaScript Type | C# Type | Go Type |
| ------------- |:-------------:|:---------------:|:-------:|:-------:|
| Int           | int           | int             | int     | int32   |
| BigInt        | long          | BigInt or string | long | int64 |
| Float         | double        |  double         | double  | float32 |
| BigFloat      | double        | BigDouble or string | double | float64 |
| String        | string        |    string       | string     | string  | 
| ID            | string        |  string         | string     | string  |
| Boolean       | bool          | boolean         | bool       | bool    |
| Date          | java.util.Date | Date           | DateTime   | time.Time |

**Note: The only differientation between ID and String above is that string should always be human readable, while ID is not required to be human readable(though that doesn't preclude it from being such)**
