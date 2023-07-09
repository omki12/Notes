Do not pass auth token in URL. Pass as **x-functions-key** header.

There are Two ways to do binding. 
1. Using function.json file.
1. using Imperative way. (declare on function parameter itself). For local development, we mostly go with the imperative way.

## Function Runtime:
There are For versions of Function runtime v1..v4. v4 is the latest one and all others are deprecated. 
With v4, .net6+ are supported now. .net3 is also depreacated with v3.

## Function Runtime Types in v4:
There are two ways we can run .net code in function app: InProcess and Isolated. 
| Item                  | In Process | Isolated |
|-----------------------|------------|----------|
| .Net                   | LTS only (3.1, 6) | All |
| Cancellation Tokens    | Supported | Not Supported |
| Core package           | Microsoft.NET.Sdk.Function | Microsoft.Azure.Functions.Worker |
| Binding Ext            | Microsoft.Azure.Webjobs.Extenstions.* | Microsoft.Azure.Functions.Worker.Extenstions.* |
| Output Binding         | Out parameter | return values |
| Output Types           | client specific | Simple types, Serializable, array | 
| Multiple Output        | Supported | Supported | 
| Imperative binding     | Supported | Not Supported |
| Function.Json artifact | Generated | Not Generated | 
| Configuration          | host.json | host.json + custom initialization | 
| DI                     | Supported | Supported |
| Durable Function       | Supported | Not supported 
| Middleware             | Not Supported | Supported | 
| Cold Start             | Typical   | Longer | 
| ReadyToRun             | Supported | Supported | 



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
**Function** is _part of_ --> **Function App**

**Function App** is _hosted on__ --> **host**

Function app are **not sticky to host** . It might change over the time as per the availibility of the VMs . (other than dedicated plan)

if DI is used, Singletone lifetime is renewed if host is changed.


this Host is defined by App Service Plan.

## Durable Function 
#### Durable Function framework: Based on open source framework: Azure Durable Task Framework
#### Task Hub: task hub is used by the durable task framework to store and track the state of orchestrator method and deliver messages between activities.
#### Orchestrator Function: Durable function runtime provides all the task management capabilities to this Function.
#### Activity function: Normal functions. Do not have Expiry time. 
#### Entity Function: It can have state. Can read and write data from Task hub.
#### Client Function: called by client, such as process outside of durable function runtime. Purpose is to raise event in durable function runtime, may to continue the excecution or cancel it.

Durable Function contains 3 types of Methods:
1. Start Method: This is the entry point to durable function workflow. whenever invoked, it generate the instance and returns to the caller. 
1. Activity Methods: This are the methods, which actully performs the tasks. 
1. Orchestrator Method: This is the heart of the durable function.
This method uses azure storage queue to persist the state of the currently running process. (Task hub) 
