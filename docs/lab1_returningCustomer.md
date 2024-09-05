# Lab 1 - Routing Returning Callers

<h1>Story</h1>
> If a customer calls back into the contact center after a dropped call, missed callback, or to continue their original call within ten minutes of the call ending, We are going to prioritize their call in the queue and deliver the call summary to the agent.



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

