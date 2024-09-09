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

---

## Build

### Create an new flow

### Create these flow variables



### Add an HTTP Request node for our query

> Select Use Authenticated Endpoint
> 
> Path: /search
> 
> Method: POST
> 
> Content Type: Application/JSON
>
> Copy this GraphQL query into the request body:
```JSON
{"query":"query refactored315($from:Long! $to:Long! $timeComparator:QueryTimeType $filter:TaskFilters){task(from:$from,to:$to,timeComparator:$timeComparator,filter:$filter){tasks{id status channelType createdTime endedTime origin destination contactReason direction owner{name id}terminationType isActive isCallback recordingLocation isRecordingDeleted lastWrapupCodeName csatScore sentiment autoCsat}}}","variables":{"from":"1725027914212","to":"1725028506363","timeComparator":"endedTime","filter":{"status":{"equals":"ended"}}}}
```
> <details><summary>Expanded Query For Understanding (optional)</summary>
```GraphQL
query refactored315(
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
      contactReason
      direction
      owner {
        name
        id
      }
      terminationType
      isActive
      isCallback
      recordingLocation
      isRecordingDeleted
      lastWrapupCodeName
      csatScore
      sentiment
      autoCsat
    }
  }
}

Variables:
{
  "from": "1725027914212",
  "to": "1725028506363",
  "timeComparator": "endedTime",
  "filter": {
    "status": {
      "equals": "ended"
    }
  }
}
```
</details>

> Parse Settings:
>
> - Content Type: JSON
>
>       - Output Variable:
>       - Path Expression:
>
> ---

### Add a Condition node

### Add a Play Message node

### Add a Queue Call node

### Add a Subflow node

### Add a Disconnect Contact node

### Add a Queue Call node

### Publish your flow

### Map your flow to the inbound channel

## Testing

