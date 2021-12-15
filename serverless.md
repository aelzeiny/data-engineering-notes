# Hidden Cost of Serverless
At LT we based an entire product on serverless. I don't mean AWS Fargates, which are like servers that hold your 
Docker container, but actually everything was run on Lambdas. Live Stream processing was done in FIFO SQS and lambdas. 
I've always been curious as to whether the serverless world just brought about new-age-less understood problems. 

The wise man who built this architecture told me something shocking: Serverless can be worse at scale than servers. Lambdas don't just go brrrr...

He said:
```
Continuing our conversation on why lambda based architecture is not a good choice when scaling needs are too high:
* After crossing a certain threshold in scaling needs, Lambda costs more than regular EC2
* When Lambda processing needs to load something from the database to complete it tasks, you will doing this loading action more than needed in the case of lambda based architecture. Part of this can be avoid through a shared cache layer like Redis. You need to choose the right balance between caching vs loading it on the fly
* With the message stream processing, we have to relay on AWS hooks to hand off the messages from the stream to these Lambdas. These hooks are not mature enough compared to writing your own stream consumers. For example, SQS hook can only read and handoff 10 messages at a time even though you have thousands of messages sitting in the queue waiting to be processed.
* Multiple runs of the same lambda instance share its static resources. This is confusing for new developers until they realize and could created a mess if not catch early on. This also an advantage if used in a proper way. It may not fall into the category of lambda scaling issue but in the category of better development practices
* Dealing with Cloudwatch logs of various lambda runs. Query insights is getting better but still a pain to work with
```