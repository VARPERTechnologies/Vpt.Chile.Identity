# Introduction

This class gives the possibility to handle RUTs and RUNs in Chile as simple as possible, giving you piece of mind when you choose this library, allowing you to worry only about business.

# Features

## RUT and RUN string parsing and validation with the static function FromString:
```
RUT rutValue = RUT.FromString("22222222-2"); // this will produce a non null rutValue
RUT rutValue = RUT.FromString("22222222-1"); // this will throw an exception due this RUT has an invalid verifier digit
```
## RUT and RUN verifier digit calculator with function FromNumber:
```
RUT rutValue = RUT.FromNumber(12345678); // this will produce a non null rutValue = 12345678-5
```
## Flexible ToString formatting:
```
RUT rutValue = RUT.FromNumber(12345678);
Console.WriteLine(rutValue.ToString()); // prints "12345678-5"
Console.WriteLine(rutValue); // prints "12345678-5" using implicit casting
Console.WriteLine(rutValue.ToString("M")); // prints "12.345.678-5" using formatting miles mark
Console.WriteLine(rutValue.ToString("N")); // prints only numeric part "12345678" using formatting number mark
Console.WriteLine(rutValue.ToString("N10")); // prints only numeric part "0012345678" using formatting number mark with zero padding
Console.WriteLine(rutValue.ToString("V")); // prints only verifier digit part "5" using formatting verifier digit mark
Console.WriteLine(rutValue.ToString("N10V")); // prints full RUT without hyphen and zero padded "00123456785"
Console.WriteLine(rutValue.ToString("N10-V")); // prints full RUT with hyphen and zero padded "0012345678-5"

Console.WriteLine(rutValue.ToString("This rut is an example wit numeric part N and verifier digit part V and miles formatting M")); 
// prints "This rut is an example wit numeric part 12345678 and verifier digit part 5 and miles formatting 12.345.678-5"

Console.WriteLine(rutValue.ToString("This will 'throw exception when trying to print N")); 
//Throws exception because single quotes are not closed. This is an special character to include literals and avoid replacing N, V or M marks

Console.WriteLine(rutValue.ToString("This will 'escape N' and not 'M' M")); 
//prints "This will escape N and not M 12.345.678-5"
```
This is useful to build custom strings with custom formats

## Easy RUT or RUN validation with IsValid function:

```
RUT.IsValid(12345678, '5'); //Returns true
RUT.IsValid("12345678-5"); //Returns true
```
## Implicit casting:

```
//Before
RUT rutValue = (RUT)"12345678-5";
//Now
RUT rutValue = "12345678-5";
```
## Json converter attribute
Added a `RutJsonConverter` class allowing to correctly serialize RUTs or RUNs to json strings.
```
public class RutTestClass
{
  public RUT Rut = new RUT(56564545, '5');
}

RutTestClass rutTest = new RutTestClass();
//Before

Console.WriteLine(JObject.FromObject(rutTest).ToString(Newtonsoft.Json.Formatting.None));
//prints {"Rut":{"Number":56564545,"VerifierDigit":"5"}}

//Now
Console.WriteLine(JObject.FromObject(rutTest).ToString(Newtonsoft.Json.Formatting.None));
//prints {"Rut":"56564545-5"}
