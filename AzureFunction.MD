Do not pass auth token in URL. Pass as **x-functions-key** header.

There are Two ways to do binding. 
1. Using function.json file.
1. using Imperative way. (declare on function parameter itself). For local development, we mostly go with the imperative way.

There are For versions of Function runtime v1..v4. v4 is the latest one and all others are deprecated. 
With v4, .net6+ are supported now. .net3 is also depreacated with v3.

There are two ways we can run .net code in function app: InProcess and Isolated. 

------------------------------------------------

Run method need not be static. 
Can use [dependency-injection] (https://learn.microsoft.com/en-us/azure/azure-functions/functions-dotnet-dependency-injection) with function using `class Startup` which inherits `FunctionsStartup`
