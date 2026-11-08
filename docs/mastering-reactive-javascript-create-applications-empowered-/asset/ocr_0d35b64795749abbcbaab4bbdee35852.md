It will receive only one mandatory parameter:

+   - `payload`: This is an object containing information to execute a command; it can have up to three properties:
+     - `from`: This property is mandatory; it is a string representing the user sending the command.
+     - `action`: This property is mandatory; it is a string representing the command (in our implementation it can be `roll_a_dice` or `echo`).
+     - `content`: This property is used only by the `echo` command; it contains the string to be echoed back to the user

Our service must implement the following rules:

+   - Must implement the `roll_a_dice` command, which is sent back to the user executing the command, a message containing a random number between 1 and 6
+   - Must implement the `echo` command, which is sent back to the user executing the command, message equal to that sent by the user
+   - All messages sent by this service must have the server as sender of the message
+   - If case a user calls the command service passing a command that is not supported, the server must respond to the user informing him this command is not supported

Now we know the rules we can implement tests for them.

# Test implementation

We will create three tests for our command service: the first will test if the `roll_a_dice` command works as expected; the second will check the `echo` command; and the last will check the response of the service when using a command that is not supported.

Create a file called `command_test.js` and paste in the following code to implement our test suite:

```
let assert = require('assert');
let mockDataSourceFactory = require('./mock_datasource_factory');
let servicesFactory = require('../src/services');

describe('Command Test',function(){
    it('Must support roll_a_dice command',function(done){
        let mockDataSource = mockDataSourceFactory();
```