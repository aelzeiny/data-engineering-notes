# API Gateway

## Serverless Framework vs AWS SAM
- I did a project in serverless framework: here are my observations
- Serverless framework is a NodeJS runtime that calls python in bash
- Extensive community & plugins for various AWS tasks
- Weak support for local development. No Support for websockets.
- With either of these you'll probably end up fighting YAML configs

## Websockets
### Requests
- The ConnectionManager endpoint is the entrypoint for opening & closing websockets.
    * This is where we "remember" connection IDs 
    * This is where we decide if the user should be allowed to upgrade from HTTPS to WSS.
    * Auth can be added here.
- HTTP "Routes" are simulated by "actions". User sends a message with the action
    * A request is formatted as a JSON like {"action": <route>, ...}
    * Each action has a different route, but really it's all the same WS route.
    * Each action has a different function in the controller.
    * the "$default" route is a catch-all for any strange request
### Response
- Remember the connection manager endpoint that passes in a connection_id? Yeah, you have to remember those
- To send a message to a user, use the AWSCLI APIGatewayManagement API & send an (authenticated via AWS) HTTP request to 1 connection ID.
- There's no broadcast capabilities.
- Each message is 1 http call
- You can also close the connection by a different HTTP call via the AWSCLI/Botocore.
### Pros & Cons
- Pro: You can hold so many simulatenous connections in theory
- Pro: Connections are held between deployments.
- Con: You must store ConnectionIDs on your side. Through SQL/REDIS/DynamoDB, you must presist these strings through a stateful backend. Redeployment means you clear the backend.
- Con: No broadcast ability. In theory posting 1000 HTTP calls to 1000 connections would take a while.
- Con: Managing Connection state is also difficult. Timeouts are set to 10 minutes, and are not very configurable.
- Con: Use must call an API to terminate connections.