# Simple Workflow Service (SWF)
[SWF FAQ](https://aws.amazon.com/swf/faqs/)

Amazon Simple Worklfow Service is a web service that enables the coordination of work distributed across application components.

Made up of tasks, which represent invoking:
- executable code
- web service calls
- human actions
- scripts

Workflow executions can last up to `1 year`. Task-oriented API vs. SQS's message-oriented API.

Tasks are assigned only once, never duplicated. SWF keeps track of all tasks and events in an application.

Main difference between SQS and SWF is that SWF involves human tasks.

SWF Actors:
- Workflow Starters - Can start a workflow.
- Deciders - Controls flow of activity tasks in workflow execution.
- Activity Workers - Carry out activity tasks.
