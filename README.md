# Vert.x 3 Kotlin bindings

This module provides [Kotlin](http://kotlinlang.org) language bindings including DSL and extension functions 
for [vert.x 3](http://vertx.io/)

# Get started (Gradle)

**TBD**

### Run simple application
```bash
./gradlew runExample
```

after that open http://localhost:8084/testit.html and try to play with queries and the [source code](src/examples/kotlin/route.kt).

## Examples

### Simple REST service

```kotlin
fun main(args: Array<String>) {
    DefaultVertx {
        httpServer(8084) { request ->
            bodyJson {
                object_(
                    "title" to "Hello, my remote peer",
                    "message" to "You address is ${request.remoteAddress().host()}"
                )
            }
        }
    }
}
```

### Simple echo socket server

```kotlin
class EchoSocket : AbstractVerticle() {
    override fun start() {
        netServer(9094) { client ->
            client.startPumpTo(client)
        }
    }
}
```

### HTTP routing example (non-apex)

```kotlin
import io.vertx.kotlin.lang.*

fun main(args: Array<String>) {
    DefaultVertx {
        httpServer(8084, "0.0.0.0", Route {
            GET("/path") { request ->
                bodyJson {
                    "field1" to "test me not"
                }
            }

            otherwise {
                setStatus(404, "Resource not found")
                body {
                    write("The requested resource was not found\n")
                }
            }
        })
    }
}
```

### More examples
See [more examples](src/examples/kotlin). Some of them just copied from original examples at vert.x repo.

