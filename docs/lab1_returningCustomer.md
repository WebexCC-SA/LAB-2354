# Lab 1 - Routing Returning Callers

## Story
> If a customer calls back into the contact center after a dropped call, missed callback, or to continue their original call within ten minutes of the call ending, we are going to prioritize their call in the queue and deliver the call summary to the agent.

### High Level Explanation
1. New call comes into the flow
2. Call the Search API to check if the ANI (caller's number) had a call which ended in the last 10 minutes
3. If the caller had a call which ended within the last 10 minutes, We will play a message and will queue the call with a higher priority so they will get assigned to the next available agent.
4. If the caller did not end a call with the contact center in the previous 10 minutes, we will queue the call normally

---

## Preconfigured Elements

1. Wait treatment Subflow which will provide Music in Queue and Queue Messages. 
2. Connector for calling Webex Contact Center APIs

---

## Build

### Create an new flow

> Create a flow named <copy><w class ="POD">yourLabID</w>_ReturningCaller</copy>

---

### Create these flow variables

> Name: <copy>previousID</copy>
>
> Type: String
>
> Default Value: empty

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
> Text-to-Speech Message: <copy>Welcome to Lab 1 of Solutions use-cases and APIs.</copy>
>
---

### Add an HTTP Request node for our query

> Select Use Authenticated Endpoint
>
> Connector: WxCC_API
> 
> Path: /search
> 
> Method: POST
> 
> Content Type: Application/JSON
>
> Copy this GraphQL query into the request body:
```JSON
{"query":"query lastTen($from:Long! $to:Long! $timeComparator:QueryTimeType $filter:TaskFilters){task(from:$from,to:$to,timeComparator:$timeComparator,filter:$filter){tasks{id status channelType createdTime endedTime origin destination direction terminationType isActive isCallback lastWrapupCodeName}}}","variables":{"from":"{{now() | epoch(inMillis=true) - 600000}}","to":"{{now() | epoch(inMillis=true)}}","timeComparator":"endedTime","filter":{"and":[{"status":{"equals":"ended"}},{"origin":{"equals":"{{NewPhoneContact.ANI}}"}},{"connectedCount":{"gte":1}}]}}}
```
> <details><summary>Expanded Query For Understanding (optional)</summary>
```GraphQL
query lastTen(
  $from: Long!
  $to: Long!
  $timeComparator: QueryTimeType
  $filter: TaskFilters
) {
  task(from: $from, to: $to, timeComparator: $timeComparator, filter: $filter) {
    tasks {
      id
      status
      channelType
      createdTime
      endedTime
      origin
      destination
      direction
      terminationType
      isActive
      isCallback
      lastWrapupCodeName
    }
  }
}
```
``` JSON
Variables:
{
  "from": "{{now() | epoch(inMillis=true) - 600000}}", # time now - 10 minutes represented in EPOCH time(ms)
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
          "equals": "{{NewPhoneContact.ANI}}" # ANI or caller phone number
        }
      },
      {
        "connectedCount": {
          "gte": 1
        }
      }
    ]
  }
}
```
</details>

> Parse Settings:
>
> Content Type: JSON
>
> - Output Variable: `previousID`
> - Path Expression: <copy>`$.data.task.tasks[0].id`</copy>
>
> ---

### Add a Condition node
> Expression: <copy>`{{previousID is empty}}`</copy>
>
> We will connect the True node in a future step.
>
> Connect the False node edge to the Play Message node created in the next step.
>
---

### Add a Play Message node
> Connect the False node edge from the previous step to this node
>
> Enable Text-To-Speech
>
> Select the Connector: Cisco Cloud Text-to-Speech
>
> Click the Add Text-to-Speech Message button
>
> Delete the Selection for Audio File
>
> Text-to-Speech Message: It looks like you were just working with an agent and had to call back in.  We are prioritizing this call for the next available agent.
>
---

### Add a Queue Contact node
> Select Static Queue
>
> Queue: <w class="Queue">yourQueueID</w>
>
> Select Static Priority
>
> Static Priority Value: P1
>
---

### Add a Subflow node

---

### Add a Disconnect Contact node

---

### Add a Queue Contact node
> Connect the True node edge from the Condition node to this node
> 
> Select Static Queue
>
> Queue: <w class="Queue">yourQueueID</w>
>
> Connect the Output node edge from this node to the Subflow node
---

### <details><summary>Check your flow</summary>![](./assets/lab1_flow.png)</details>

---

### Publish your flow
> Turn on Validation at the bottom right corner of the flow builder
>
> If there are no Flow Errors, Click Publish
>
> Add a publish note
>
> Add Version Label(s): Live 
>
> Click Publish Flow

---

### Map your flow to your inbound channel
> Navigate to Control Hub > Contact Center > Channels
>
> 


---

## Testing

<script src='../assets/load.js'><script>