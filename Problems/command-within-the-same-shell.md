[Metadata]
title: Command Within The Same Shell
date:  2014-08-16 18:46:00

[Tags]
difficulty: 1
categories: pipeline, shell
source: unknown

[Description]
Suppose that you have to write a script to receive argument from command line, which is another command.
The command which passed as argument will run in the current shell and return some results.

For example, you have to list all the process when you start a shell, of course including the shell process.
And do some other things with the results.

Run by

```
$./example.sh 'ps aux'
```

In shell script

```bash
#!/bin/bash

#How to run the command
```

contribute by: violet

[Solution]
Use pipeline with echo

```bash
#!/bin/bash

echo $* | bash

#do something else
```

