# Vegeta

Vegeta is a versatile HTTP load testing tool built out of need to drill
HTTP services with a constant request rate.

![Vegeta](https://dl.dropboxusercontent.com/u/83217940/vegeta.png)

## Install
You need go installed and `GOBIN` in your `PATH`. Once that is done, run the
command:
```shell
$ go install github.com/tsenart/vegeta
```

## Usage
```shell
$ vegeta -h
Usage of vegeta:
  -duration=10s: Duration of the test
  -ordering="random": Attack ordering [sequential, random]
  -output="stdout": Reporter output file
  -rate=50: Requests per second
  -reporter="text": Reporter to use [text]
  -targets="targets.txt": Targets file
```

#### -duration
Specifies the amount of time to issue request to the targets.
The internal concurrency structure's setup has this value as a variable.
The actual run time of the test can be longer than specified due to the
responses delay.

#### -ordering
Specifies the ordering of target attack. The default is `random` and
it will randomly pick one of the targets per request without ever choosing
that target again.
The other option is `sequential` and it does what you would expect it to
do.

#### -output
Specifies the output file to which the report will be written to.
The default is stdout.

####  -rate
Specifies the requests per second rate to issue against
the targets. The actual request rate can vary slightly due to things like
garbage collection, but overall it should stay very close to the specified.

#### -reporter
Specifies the reporting type to display the results with.
The default is a text report printed to stdout.

#### -targets
Specifies the attack targets in a line sepated file. The format should
be as follows:
```
GET http://goku:9090/path/to/dragon?item=balls
HEAD http://goku:9090/path/to/success
...
```

#### Limitations
There will be an upper bound of the supported `rate` which varies on the
machine being used.
You could be CPU bound (unlikely), memory bound (more likely) or
have system resource limits being reached which ought to be tuned for
the process execution. The important limits for us are file descriptors
and processes. On a UNIX system you can get and set the current
soft-limit values for a user.
```shell
$ ulimit -n # file descriptors
2560
$ ulimit -u # processes / threads
709
```
Just pass a new number as the argument to change it.

## TODO
* Add timeout options to the requests
* Use TabWriter in TextReporter
* Graphical reporters
* Cluster mode (to overcome single machine limits)
* More tests
* HTTPS

## Licence
See the `LICENSE` file.