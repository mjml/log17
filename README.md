# Log17
Log17 is a small C++17 header for static logging.

**Static logging** is when you associate a singleton `Log` class with some set of message-generating code,
  typically a single module or a set of grouped modules which share this `Log`.
It is *static* because the number of `Log`s doesn't change and hence the class can use static methods
  for dispatching messages.
No object instantiation is necessary.
This library uses partial template specialization of a single class to provide both logs and sinks.

The API comes with a built-in `FILE*` sink, but customizing it for other i/o (e.g. `std::iostream`) is rudimentary.
The `FILE*` sink is enough to pass in stdio, file, and `popen()` handles.

You can then instantiate a singleton `Log` classes which accept any number of other
  singleton `Log` classes as template parameters.
These parameters act as sinks for the prior `Log`, allowing you to construct entire graphs
  of `Log` objects.

For example, you can have a main application `Log` that's tied to a `FILE*`-based `Log` acting as a sink,
  and another `FILE*`-based `Log` which writes to a file.
When you write to the application `Log`, it writes to both of its underlying sinks.
On top of that, you can have feature or module Log which uses the main application `Log` as *its* sink.

Each `Log` comes with nine levels of verbosity and can be configured by a
  template parameter (and hence via preprocessor macro) or at runtime.
Using preprocessor macros as template arguments allows you to tune the verbosity of logging code
  at compile time.
If you configure your Logger's verbosity at compile time, then messages that exceed its threshold
  are nooped, resulting in very fast code.
Never throw away your debugging messages again!
Just put them in a "feature" `Log` and turn that log's verbosity down when you don't need it.
