Using scala [macro annotation][mcr] to help loading [config][conf].

## Example

```
import com.wacai.config.annotation._

class Server {
  @conf val port = 0
}
```

```
// application.conf

server {
  port = 8080
}
```

`@conf` will let scala compile to transform `val port = 0` to :

```
val port = config.getInt("server.port")
```

## Installation

Set up your `build.sbt` with:

```
resolvers += Resolver.sonatypeRepo("releases")

addCompilerPlugin("org.scalamacros" % "paradise" % "2.0.1" cross CrossVersion.full)

libraryDependencies += "com.wacai" %% "config-annotation" % "0.1"

```

## Path covenant

|Scala definition | Config path |
|-----------------|-------------|
|Server.port      | server.port  |
|HttpServer.port  | http_server.port|
|Server.maxBufferSize| server.max.buffer.size|

## Type covenant

|Scala type | Config getter |
|-----------|---------------|
| Boolean   | getBoolean    |
| Int       | getInt        |
| Long      | getBytes      |
| Double    | getDouble     |
| String    | getString     |
| Duration  | getDuration   |


## Integrate with akka actor

```
import com.wacai.config.annotation._

class MyActor extends Actor with Configurable {
  val config = context.system.settings.config

  @conf val threshold = 0

  def receive = ???

}
```

## More detail

Please see test cases.


[mcr]:http://docs.scala-lang.org/overviews/macros/annotations.html
[conf]:https://github.com/typesafehub/config