# Lab 2 - Last Agent Routing Using Simulated AutoCSAT

## Story

### High Level Explanation

## Preconfigured elements
1. Wait treatment Subflow which will provide Music in Queue and Queue Messages. 
2. Connector for calling Webex Contact Center APIs
3. Agent Editable, Reportable Global Variable

# Build
### Create an new flow

> Create a flow named <w>yourLabID</w>_LARwCSAT 

### Create these flow variables

> Name: agentID
>
> Type: String
>
> Default Value: empty

---

> Name: queriedCSAT
>
> Type: Decimal
>
> Default Value: 0.0

---

### Add the Global Variable simulatedCSAT to the flow
> There are no values to set because it has already been configured globally

---

### Add a Play Message node for our welcome message

> Connect the New Phone Contact to this Play Message node
>
> Enable Text-To-Speech
>
> Select the Connector: Cisco Cloud Text-to-Speech
>
> Click the Add Text-to-Speech Message button
>
> Delete the Selection for Audio File
>
> Text-to-Speech Message: Welcome to Lab 2 of Solutions use-cases and APIs.
>
---


### Add an HTTP Request node for our query

> Select Use Authenticated Endpoint
>
> Connector: 
> 
> Path: /search
> 
> Method: POST
> 
> Content Type: Application/JSON
>
> Copy this GraphQL query into the request body:
```JSON
{"query":"query simulatedCSAT($from:Long! $to:Long! $timeComparator:QueryTimeType $filter:TaskFilters $name:String!){task(from:$from,to:$to,timeComparator:$timeComparator,filter:$filter){tasks{owner{name id}simulatedCSAT:doubleGlobalVariables(name:$name){name value}}}}","variables":{"from":"{{now() | epoch(inMillis=true) - 604800000}}","to":"{{now() | epoch(inMillis=true)}}","timeComparator":"endedTime","filter":{"and":[{"status":{"equals":"ended"}},{"origin":{"equals":"{{NewPhoneContact.ANI}}"}},{"doubleGlobalVariables":{"name":{"equals":"simulatedCSAT"},"value":{"gte":3}}}]},"name":"simulatedCSAT"}}
```
> <details><summary>Expanded Query For Understanding (optional)</summary>
```GraphQL
query simulatedCSAT(
  $from: Long!
  $to: Long!
  $timeComparator: QueryTimeType
  $filter: TaskFilters
  $name: String!
) {
  task(from: $from, to: $to, timeComparator: $timeComparator, filter: $filter) {
    tasks {
      owner {
        name #Agent Name
        id #Agent ID
      }
      simulatedCSAT: doubleGlobalVariables(name: $name) {
        name
        value #Value of the simulatedCSAT
      }
    }
  }
}
```
```JSON
Variables:

{
  "from": "{{now() | epoch(inMillis=true) - 604800000}}", # time now - 1 week represented in EPOCH time(ms)
  "to": "{{now() | epoch(inMillis=true)}}", # time now represented in EPOCH time(ms)
  "timeComparator": "endedTime",
  "filter": {
    "and": [
      {
        "status": {
          "equals": "ended"
        }
      },
      {
        "origin": {
          "equals": "{{NewPhoneContact.ANI}}"
        }
      },
      {
        "doubleGlobalVariables": { #Filtering on the Global Variable simulatedCSAT to be greater or equal to 3 
          "name": {
            "equals": "simulatedCSAT" 
          },
          "value": {
            "gte": 3
          }
        }
      }
    ]
  },
  "name": "simulatedCSAT" #The Alias name used for the global variable in the returned fields
}
```
</details>

> Parse Settings:
>
> Content Type: JSON
>
> - Output Variable: `agentID`
> - Path Expression: `$.data.task.tasks[0].owner.id`
>
> - Output Variable: `queriedCSAT`
> - Path Expression: `$.data.task.tasks[0].simulatedCSAT.value`
>
---

### Add a Condition node
> Expression: `{{agentID is empty}}`
>
> We will connect the True node in a future step.
>
> Connect the False node edge to the Queue To Agent node created in the next step.
>
---

### Add a Queue To Agent node
> Agent Variable: agentID
>
> Agent Lookup Type: ID
>
> Set Contact Priority: True
>
> Select Static Priority
>
> Static Priority Value: P1
>
> Reporting Queue: yourQueueID
>
> Park Contact if Agent Unavailable: False
>
> Recovery Queue: yourQueueID
>
> Connect the Output and Error node edges to the Queue Contact node created in the next step
---

### Add a Queue Contact node
> Connect the True node edge from the Condition node to this node
> 
> Select Static Queue
>
> Queue: <w>yourQueueID</w>
>
> Connect the Output node edge from this node to the Subflow node
---

### Add a Subflow node

---

### Add a Disconnect Contact node

---

# Testing