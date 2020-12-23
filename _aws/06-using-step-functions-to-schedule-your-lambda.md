---
title: Using Step Functions to Schedule Your Lambda
permalink: /aws/06-using-step-functions-to-schedule-your-lambda.html
layout: single
read_time: true
author_profile: true
share: true
comments: true
excerpt: "Have you come across a time where you wanted to run a lambda function at a particular time or set of lambda functions at particular intervals?"
header:
  overlay_image: /assets/images/aws/06-using-step-functions-to-schedule-your-lambda/step-functions-to-schedule-lambda.jpg
  teaser: /assets/images/aws/06-using-step-functions-to-schedule-your-lambda/step-functions-to-schedule-lambda.jpg
  overlay_filter: 0.5
last_modified_at: 2019-05-30T15:59:07-04:00
toc: true
sidebar:
nav: aws
---

Have you come across a time where you wanted to run a lambda function at a particular time or a sequence of lambda functions at particular intervals? I came across this problem in one of our projects where we needed to control the execution according to the start time and end time of an online classroom. So how can you execute your lambda at a particular time or how can you keep a time interval between the execution of sequence of lambdas? Aws provide a solution to these using [AWS Step Functions](https://aws.amazon.com/step-functions/). While Step Functions provide a wide variety of uses, scheduling your lambda function to given time or time interval is a key use.

### What is a Step Function? ###
Step Functions are a way of controling workflow in aws. It lets you control and stitch together multiple aws resources with a series of steps. 

### Example Scenario ###
What we will be discussing in this article is an online classroom. When an user creates a classroom, two separate lambda functions should send data to the classroom server at the start time and end time respectively. We will use a step function to build this process.

### Define the step function ###
Before executing the step function we need to define a state machine using Step Functions console or **serverless.yml** if you are using serverless framework. 

If you use the console you can follow this [link](https://docs.aws.amazon.com/step-functions/latest/dg/tutorial-creating-lambda-state-machine.html#create-lambda-state-machine-step-4) to create the state machine. Use the state machine definition below in the State machine definition pane. 

If you are using serverless framework you can use the following definition in your **serverless.yml**.

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
            Next: SendLivePingWait
          SendLivePingWait:
            Type: Wait
            Seconds: 60
            Next: StoppingWait
          SendLivePing:
            Type: Task
            Resource: arn:aws:lambda:ap-southeast-1:13######:function:consult-api-dev-SendLivePing
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

Let's go through this definition. First you have to define this under stepFunctions in the serverless.yml file. You can define a http event handler if you want but this is optional. 
```yml
events:
        - http:
            path: classroom/create
            method: POST
```

Give a name to the state machine (`name: classroom-state-machine`). In the state machine you can define several **states**. Each state would mean either a task or time interval (`Type: Task/Wait`). A typical [task](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-tasks.html) would be a lambda function. A time interval or a [wait](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-wait-state.html) step is to define time periods between the execution of tasks. This is what comes in handy when you want to schedule a lambda. In a task you need to provide the ARN of the resource (i.e. ARN of lambda function) and what would be the next state (`Next: StartRecording`)that would be executed.

In the definition, first you have to define at what state the execution of the state machine should **begin** (`StartAt: RecordingWait`) and in one of the states you should define where it **ends** by giving `End:true`. Flow of the state machine would be defined by `Next` and `End` statements in the state. 

### Execute the Step Function ###

We have now defined the state machine in serverless framework. Now you can execute it in a lambda or any other function. Given below is the function used to start the execution of this step function.

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
Here in `Param` which we passed to the `startExecution` method of the step function, we need to give the arn of the state machine, **data object** that we would be sending to the start state of the state machine and the name of the state machine to identify a particular execution. Giving an **unique name** to the execution would be helpful since we can see the **current state** in the visual flow diagram of the state machine in step functions console. Here we have used the classroom id as the execution name.

### Explaining the example ###
The above definition is of a online classroom where recording of the classroom should begin at the start time of the class, send a live status to the classroom and recording should stop at the class end time. The execution begins at `RecordingWait` state which is a wait step and it waits until `start` which is the **timestamp** of the class start time. Here `$.start` means start time we passed at the start of the execution. Normally, a classroom is scheduled at least a day prior to the class start time. At the time of class creation we would start the execution of the state machine. The state machine would wait until the classroom start time to start the recording. This is the functionality that the state machine brings in. 

At the class start time, the state machine would move to the next state which is `StartRecording` which is a lambda function to start the recorder. After starting the recorder it will move on to the next state which is `SendLivePingWait` which is a wait step. This is a bit different to the first wait step since here the time period would be defined in **seconds** and not timestamp. So the state machine would wait 60 seconds as per this example before moving on to the next state which is to execute the task `SendLivePing`. After executing the `SendLivePing` state it would again wait till the end of class to stop the recorder. At the end time it will execute the `StopRecorder` task to stop the recorder and it will end the state machine there since we have defined to end the state machine there.

P.S. : To pass data after the execution of a lambda to the next state, the data should be returned in the callback of the lambda function.