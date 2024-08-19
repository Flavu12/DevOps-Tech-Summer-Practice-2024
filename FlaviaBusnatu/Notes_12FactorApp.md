# The Twelve-Factor App
In the modern era, software is commonly delivered as a service: called web apps, or software-as-a-service. The twelve-factor app is a methodology for building software-as-a-service apps  
The twelve-factor methodology can be applied to apps written in any programming language, and which use any combination of backing services (database, queue, memory cache, etc).  
## 1. Codebase  
### One codebase tracked in revision control , many deploys  
   A codebase is any single repo, or any set of repos who share a root commit  
   There is always a one-to-one correlation between the codebase and the app:
   - If there are multiple codebases, it’s not an app – it’s a distributed system
   - Multiple apps sharing the same code is a violation of twelve-factor. The solution here is to factor shared code into libraries which can be included through the dependency manager

There is only one codebase per app, but there will be many deploys of the app. A deploy is a running instance of the app.
## 2. Dependencies  
### Explicitly declare and isolate dependencies
Most programming languages offer a packaging system for distributing support libraries.Libraries installed through a packaging system can be installed system-wide (known as “site packages”) or scoped into the directory containing the app (known as “vendoring” or “bundling”).  

One benefit of explicit dependency declaration is that it simplifies setup for developers new to the app  
## 3. Config  
### Store config in the environment
   An app’s config is everything that is likely to vary between deploys (staging, production, developer environments, etc). This includes:  
      - Resource handles to the database, Memcached, and other backing services
      - Credentials to external services
      - Per-deploy values such as the canonical hostname for the deploy  

Apps sometimes store config as constants in the code. This is a violation of twelve-factor, which requires strict separation of config from code. Config varies substantially across deploys, code does not.  

The twelve-factor app stores config in environment variables (often shortened to env vars or env). Env vars are easy to change between deploys without changing any code  
## 4. Backing services  
### Treat backing services as attached resources
A backing service is any service the app consumes over the network as part of its normal operation  

Backing services like the database are traditionally managed by the same systems administrators who deploy the app’s runtime.  

The code for a twelve-factor app makes no distinction between local and third party services. To the app, both are attached resources, accessed via a URL or other locator/credentials stored in the config.  
## 5. Build, release, run   
### Strictly separate build and run stages
   A codebase is transformed into a (non-development) deploy through three stages:
   - The build stage is a transform which converts a code repo into an executable bundle known as a build.
   - The release stage takes the build produced by the build stage and combines it with the deploy’s current config.
   - The run stage (also known as “runtime”) runs the app in the execution environment, by launching some set of the app’s processes against a selected release.
  
The twelve-factor app uses strict separation between the build, release, and run stages. For example, it is impossible to make changes to the code at runtime, since there is no way to propagate those changes back to the build stage.
 
Builds are initiated by the app’s developers whenever new code is deployed. Runtime execution, by contrast, can happen automatically in cases such as a server reboot, or a crashed process being restarted by the process manager.
## 6. Processes  
### Execute the app as one or more stateless processes
The app is executed in the execution environment as one or more processes.  
In the simplest case, the code is a stand-alone script, the execution environment is a developer’s local laptop with an installed language runtime  
 
Twelve-factor processes are stateless and share-nothing. The data that needs to persist should be stored in a backing service like a database.  
 
The memory space or filesystem of the process can be used as a brief, single-transaction cache. For example, downloading a large file, operating on it, and storing the results of the operation in the database.  
 
The twelve-factor app never assumes that anything cached in memory or on disk will be available on a future request or job
## 7.Port Binding  
### Export services via port binding
 
The twelve-factor is completely self-contained and does not rely of a webserver  
 
In deployment, a routing layer handles routing requests from a public-facing hostname to the port-bound web processes.  
 
This is typically implemented by using dependency declaration to add a webserver library to the app.  
 
Note also that the port-binding approach means that one app can become the backing service for another app, by providing the URL to the backing app as a resource handle in the config for the consuming app  

## 8. Concurrency
### Scale out via the process model  
 
Any computer program, once run , is represented by one or more processes.
 
In Java processes the JVM providing one massive uberprocess that reserves a large block of system resources (CPU and memory) on startup, with concurrency managed internally via threads.  
 
In the twelve-factor app, processes are a first class citizen . They can follow a process model such as the HTTP request, which may be handled by a web process, and long-running background tasks , and long-running background tasks handled by a process worker .  
 
This does not exclude individual processes from handling their own internal multiplexing, via threads inside the runtime VM. But an individual VM can only grow so large (vertical scale), so the application must also be able to span multiple processes running on multiple physical machines.  

## 9. Disposability
### Maximize robustness with fast startup and graceful shutdown
The twelve-factor app’s processes are disposable, meaning they can be started or stopped at a moment’s notice. This facilitates fast elastic scaling, rapid deployment of code or config changes, and robustness of production deploys.  
 
Processes should strive to minimize startup time. Ideally, a process takes a few seconds from the time the launch command is executed until the process is up and ready to receive requests or jobs.  
 
Processes shut down gracefully when they receive a SIGTERM signal from the process manager. For a web process, graceful shutdown is achieved by ceasing to listen on the service port (thereby refusing any new requests), allowing any current requests to finish, and then exiting.    
 
For a worker process, graceful shutdown is achieved by returning the current job to the work queue.  
 
Processes should also be robust against sudden death, in the case of a failure in the underlying hardware.  
 
## 10.Dev/prod parity
### Keep development, staging, and production as similar as possible  
 
The twelve-factor app is designed for continuous deployment by keeping the gap between development and production small  

Backing services, such as the app’s database, queueing system, or cache, is one area where dev/prod parity is important. Many languages offer libraries which simplify access to the backing service, including adapters to different types of services.  
 
The twelve-factor developer resists the urge to use different backing services between development and production, even when adapters theoretically abstract away any differences in backing services. Differences between backing services mean that tiny incompatibilities crop up, causing code that worked and passed tests in development or staging to fail in production.  
 
## 11. Logs
 
### Treat logs as event streams
 
Logs provide visibility into the behavior of a running app. In server-based environments they are commonly written to a file on disk (a “logfile”); but this is only an output format.  
Logs are the stream of aggregated, time-ordered events collected from the output streams of all running processes and backing service  
A twelve-factor app never concerns itself with routing or storage of its output stream. It should not attempt to write to or manage logfiles.  
 
The event stream for an app can be routed to a file, or watched via realtime tail in a terminal.  
 
These systems allow for great power and flexibility for introspecting an app’s behavior over time, including:
 
Finding specific events in the past.
Large-scale graphing of trends (such as requests per minute).
Active alerting according to user-defined heuristics (such as an alert when the quantity of errors per minute exceeds a certain threshold).  
 
## 12. Admin processes
### Run admin/management tasks as one-off processes
 
 
 
The process formation is the array of processes that are used to do the app’s regular business (such as handling web requests) as it runs. Separately, developers will often wish to do one-off administrative or maintenance tasks for the app, such as:  
- Running database migrations (e.g. manage.py migrate in Django, rake db:migrate in Rails).  
- Running a console (also known as a REPL shell)  
- Running one-time scripts committed into the app’s repo (e.g. php scripts/fix_bad_records.php).  
 
One-off admin processes should be run in an identical environment as the regular long-running processes of the app.  
 
The same dependency isolation techniques should be used on all process types. For example, if the Ruby web process uses the command bundle exec thin start, then a database migration should use bundle exec rake db:migrate.  
 
Twelve-factor strongly favors languages which provide a REPL shell out of the box, and which make it easy to run one-off scripts. In a local deploy, developers invoke one-off admin processes by a direct shell command inside the app’s checkout directory.  
 