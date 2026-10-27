```
output.text += 'Received from isolate: $msg\n';
}
});

String workerUri = 'worker.dart';
int counter = 0;
// start 3 isolates:
for (int i = 1; i <= 3; i++) {
  Isolate.spawnUri(Uri.parse(workerUri), [], receivePort.sendPort).
    then((isolate) {
      print('isolate spawned');
      newTimer.periodic(const Duration(seconds: 1), (t) {
        sendPort.send('From main app: ${counter++}');
        if (counter == 10) {
          sendPort.send('END');
          t.cancel();
        }
      });
    });
  }
}
```

The following is the code for the isolate worker.dart:

```
import 'dart:isolate';

main(List<String>args, SendPort sendPort) {
  ReceivePort receivePort = new ReceivePort();
  sendPort.send(receivePort.sendPort);

  receivePort.listen((msg) {
    if (msg == 'END') receivePort.close;
    sendPort.send('ECHO: $msg');
  });
}
```