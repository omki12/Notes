Do not pass auth token in URL. Pass as **x-functions-key** header.

There are Two ways to do binding. 
1. Using function.json file.
1. using Imperative way. (declare on function parameter itself). For local development, we mostly go with the imperative way.

## Function Runtime:
There are For versions of Function runtime v1..v4. v4 is the latest one and all others are deprecated. 
With v4, .net6+ are supported now. .net3 is also depreacated with v3.

## Function Runtime Types in v4:
There are two ways we can run .net code in function app: InProcess and Isolated. 

## Types of Plan
| Plan | Shared VM | Billing | Expiry time |
|------|-----------|---------|-------------|
| Consumption plan | YES | as per executions | 5 Min |
| Premium plan | YES | as per executions | 15 Min |
| Dedicated plan | No | as per VM size | None |

1. . (Host VM is shared |  billing per execution)
1. Premium plan. (Host VM is shared, billing per execution)
1. Dedicated plan/App Service (Host VM is dedicated, billing per execution)
------------------------------------------------

Run method need not be static. 
Can use [dependency-injection] (https://learn.microsoft.com/en-us/azure/azure-functions/functions-dotnet-dependency-injection) with function using `class Startup` which inherits `FunctionsStartup`

## Structure
**Function** is _part of_ -> **Function App**
Function App _hosted_ -> host

Function app are not sticky to host. It might change over the time as per the availibility of the VMs . (other than dedicated plan)
if DI is used, Singletone lifetime is renewed if host is changed.


this Host is defined by App Service Plan.
