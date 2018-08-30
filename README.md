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
