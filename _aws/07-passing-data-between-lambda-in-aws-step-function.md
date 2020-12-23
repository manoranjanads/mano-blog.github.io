---
title: Passing data between lambda in AWS Step Function
permalink: /aws/07-passing-data-between-lambda-in-aws-step-function.html
layout: single
read_time: true
author_profile: true
share: true
comments: true
excerpt: "At execution of the step function you are allowed to pass in data but how do we handle them and pass it onto lambdas on the sequence with more data"
header:
  overlay_image: /assets/images/aws/07-passing-data-between-lambda-in-aws-step-function/passing-data-beween-lambda.jpg
  teaser: /assets/images/aws/07-passing-data-between-lambda-in-aws-step-function/passing-data-beween-lambda.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-16T22:59:07-04:00
toc: true
sidebar:
nav: aws
---

We know at the execution of a step function you are allowed to pass in data to the step function. But if you have a sequence of lambdas which depend on the passed in data plus some other data that sub sequent lambdas would need to add up, that's what I'm gonna explain in this blog. 
The example we are using in this article is related to an `online classroom` where the recording of the class is handled by a step function.

### Sample Step function ###

``` yml
stepFunctions:
  stateMachines:
    stepFunction:
      events:
        - http:
            path: classroom/create
            method: POST
      name: classroom-state-machine
      definition:
        StartAt: RecordingWait
        States:
          RecordingWait:
            Type: Wait
            TimestampPath: "$.start"
            Next: StartRecording
          StartRecording:
            Type: Task
            Resource: arn:aws:lambda:ap-southeast-1:13######:function:consult-api-dev-StartRecorder
            Next: StoppingWait
          StoppingWait:
            Type: Wait
            TimestampPath: "$.end"
            Next: StopRecording
          StopRecording:
            Type: Task
            Resource: arn:aws:lambda:ap-southeast-1:13######:function:consult-api-dev-StopRecorder
            End: true
```

What step function does is that it waits and executes the `startRecorder` lambda at the start time of the classroom. It wait again till the end of classroom and executes the 'stopRecorder' lambda.

### Executing a step function ###

```javascript
const AWS = require('aws-sdk');
const stepFunction = new AWS.StepFunctions();

function executeRecordingStateMachine(start, end, className) {
    const Param = {
      stateMachineArn: RECORDER_STATE_MACHINE_ARN,
      input: JSON.stringify({ start, end }),
      name: className
    };
    return new Promise((resolve, reject) => {
      stepFunction.startExecution(Param, (err, data) => {
        if(err) {
          reject(err);
        } else {
          resolve(data);
        }
      });
    });
}
```
So here we pass in the start time and end time of the classroom to the step function at execution. Whatever the first state will receive these data as input. If the state is a **Wait** step it would automatically pass in the received data to the next state,  but if it is a lambda you need to return it in the callback. Since we have a wait step at the beginning it will receive the passed in data and will pass them back to next state which is to execute the `startRecorder` lambda.

### Receiving data in lambda ###
`startRecorder`lambda will receive both start time and end time that we passed in at the execution. They can be accessed in the function by,

```javascript
    module.exports.create = async function (event, context, callback) {
        const data = event;
        const startTime = data.start;
        const endTime = data.end;
    }
```

### Passing data to next lambda function
The `stopRecorder` lambda on the sequence will require the initial end time of classroom and an extra status about whether recording start was successfull or not. What the next state will receive is what we return in the callback of the lambda function.

```javascript
    callback(null, {...data, isRecording: true});
```
Here we have spread the initial data and also ammended `isRecording` boolean to the return object. Since the next state is a wait step it would pass this data object to the next state which is the `stopRecorder` lambda. So in `stopRecorder` lambda we can access the passed in data object as,

```javascript
    const endTime = event.end;
    const recordingStatus = event.isRecording;
```
Passing these additional variables could be useful in making a **Choice** state where the choice would depend on the result of a lambda.