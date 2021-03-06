
# Hazelcast Simulator

Hazelcast Simulator is a production simulator used to test Hazelcast and Hazelcast-based applications in clustered environments. It also allows you to create your own tests and perform them on your Hazelcast clusters and applications that are deployed to cloud computing environments. In your tests, you can provide any property that can be specified on these environments (Amazon EC2, Google Compute Engine(GCE), or your own environment): properties such as hardware specifications, operating system, Java version, etc.

Hazelcast Simulator allows you to add potential production problems, such as real-life failures, network problems, overloaded CPU, and failing nodes to your tests. It also provides a benchmarking and performance testing platform by supporting performance tracking and also supporting various out-of-the-box profilers.

Hazelcast Simulator makes use of Apache jclouds&reg;, an open source multi-cloud toolkit that is primarily designed for testing on the clouds like Amazon EC2 and GCE.

You can use Hazelcast Simulator for the following use cases:

- In your pre-production phase to simulate the expected throughput/latency of Hazelcast with your specific requirements.
- To test if Hazelcast behaves as expected when you implement a new functionality in your project.
- As part of your test suite in your deployment process.
- When you upgrade your Hazelcast version.

Hazelcast Simulator is available as a downloadable package on the Hazelcast <a href="http://www.hazelcast.org/download" target="_blank">web site</a>. Please refer to the [Installing Simulator section](#installing-simulator) for more information.

## Key Concepts

The following are the key concepts mentioned with Hazelcast Simulator.

- **Test** - A test class for the functionality you want to test, e.g. a Hazelcast map. This test class looks similar to a JUnit test, but it uses custom annotations to define methods for different test phases (e.g. `@Setup`, `@Warmup`, `@Run`, `@Verify`).

- **TestSuite** - A property file that contains the name of the `Test` class and the properties you want to set on that `Test` class instance. A `TestSuite` contains one or multiple tests. It can also contain the same `Test` class with different names and configurations.

- **Worker** - This term `Worker` is used twice in Simulator. 

  - **Simulator Worker** - A Java Virtual Machine (JVM) responsible for running the configured `Tests`. It can be configured to spawn a Hazelcast client or member instance, which is used in the tests. We refer to this `Worker` in the context of a Simulator component like `Agent` and `Coordinator`.
  
  - **Test Worker** - A Runnable implementation to increase the test workload by spawning several threads in each `Test` instance. We refer to this `Worker` in the context of a `Test`, e.g. how many worker threads a `Test` should create.

- **Agent** - A JVM responsible for managing client and member `Workers`. There is always one `Agent` per physical machine, no matter how many `Workers` are spawned on that machine. It serves as communication relay for the `Coordinator` and monitoring instance for the `Workers`.

- **Coordinator** - A JVM that can run anywhere, such as on your local machine. The `Coordinator` is actually responsible for running the `TestSuite` using the `Agents` and `Workers`. You configure it with a list of `Agent` IP addresses, and you run it by executing a command like "run this testsuite with 10 member worker and 100 client worker JVMs for 2 hours".

- **Provisioner** - Spawns and terminates cloud instances, and installs Hazelcast Simulator on the remote machines. It can be used in combination with EC2 (or any other cloud), but it can also be used in a static setup, such as a local machine or a cluster of machines in your data center.

- **Failure** - An indication that something has gone wrong. Failures are picked up by the `Agent` and sent back to the `Coordinator`.

- **simulator.properties** - The configuration file you use to adapt the Hazelcast Simulator to your business needs (e.g. cloud provider, SSH username, Hazelcast version, Java profiler settings).
