# 🐻‍❄️🌂 Noelware Analytics for JVM
> *JVM support for using [Noelware Analytics](https://analytics.noelware.org) for your applications.*
>
> <kbd><a href="https://github.com/Noelware/analytics-jvm/releases/v0.1-beta">v0.1-beta</a></kbd> | [:scroll: **Documentation**](https://analytics.noelware.org/docs/support/jvm)

**Noelware Analytics for JVM** (`analytics-jvm`) is a library to support client and server support of the Noelware Analytics
[protocol](https://analytics.noelware.org/docs/protocol) for the JVM with Kotlin support.

The library comes in a form of plugins to plug in external sources to include in your Noelware Analytics instance that can be visualised
on the web dashboard.

## Usage
### Client
You will need the instance service token to use the client for yourself.

```java
// Add the "org.noelware.analytics:client:<VERSION>" to the project before
// copying.

import org.noelware.analytics.jvm.client.blocking.BlockingAnalyticsClient;
import org.noelware.analytics.jvm.client.responses.ConnectionAckResponse;
import org.noelware.analytics.jvm.client.responses.InstanceStatsResponse;
import org.noelware.analytics.jvm.client.query.QueryBuilder;

// The client also supports asynchronous clients that use the CompletableFuture
// API. This will only demonstrate the blocking APIs.
public class Program {
    public static void main(String[] args) {
        final BlockingAnalyticsClient client = new BlockingAnalyticsClient.Builder()
                .withServiceToken("<token from dashboard>")
                .withChannelBuilder(builder -> {
                    // builder => the gRPC channel builder
                    builder.usePlaintext();
                })
                .build();

        final ConnectionAckResponse res = client.connect();
        System.out.println("connected? " + res.connected() ? "yes" : "no");

        final InstanceStatsResponse stats = client.collectStats();
        System.out.println(stats);
    }
}
```

### Server
You will still need your instance service token to validate requests from incoming traffic. The service token holds as a way to
block traffic that isn't from any client that doesn't have the service token available.

```java
import org.noelware.analytics.jvm.server.extensions.jvm.JvmMemoryPoolExtension;
import org.noelware.analytics.jvm.server.extensions.jvm.JvmThreadsPluginExtension;
import org.noelware.analytics.jvm.server.AnalyticsServerBuilder;
import org.noelware.analytics.jvm.server.AnalyticsServer;

public class Program {
    public static void main(String[] args) {
        final AnalyticsServer server = new AnalyticsServerBuilder(55123)
                .withExtensions(new JvmMemoryPoolPlugin(), new JvmThreadsPlugin())
                .withServiceToken("<service token from dashboard>")
                .build();
        
        server.start();
    }
}
```

## Installation
### Gradle (Groovy)
```groovy
repositories {
    maven 'https://maven.noelware.org'
    gradlePluginPortal()
    mavenCentral()
}

dependencies {
    implementation 'org.noelware.analytics:bom:<VERSION>'
}
```

### Gradle (Kotlin DSL)
```kotlin
repositories {
    maven("https://maven.noelware.org")
    gradlePluginPortal()
    mavenCentral()
}

dependencies {
    implementation("org.noelware.analytics:analytics-bom:<VERSION>")
    implementation("org.noelware.analytics:client")
}
```

### Maven
```xml
<repositories>
    <repository>
        <url>https://maven.noelware.org</url>
    </repository>
</repositories>
<dependencies>
    <dependency>
        <group>org.noelware.analytics</group>
        <artifactId>analytics-bom</artifactId>
        <version>{VERSION}</version>
        <type>pom</type>
    </dependency>
</dependencies>
```

## Contributing
Thanks for considering contributing to **analytics-jvm**! Before you boop your heart out on your keyboard ✧ ─=≡Σ((( つ•̀ω•́)つ, we recommend you to do the following:

- Read the [Code of Conduct](./.github/CODE_OF_CONDUCT.md)
- Read the [Contributing Guide](./.github/CONTRIBUTING.md)

If you read both if you're a new time contributor, now you can do the following:

- [Fork me! ＊*♡( ⁎ᵕᴗᵕ⁎ ）](https://github.com/Noelware/analytics-jvm/fork)
- Clone your fork on your machine: `git clone https://github.com/your-username/analytics-jvm`
- Create a new branch: `git checkout -b some-branch-name`
- BOOP THAT KEYBOARD!!!! ♡┉ˏ͛ (❛ 〰 ❛)ˊˎ┉♡
- Commit your changes onto your branch: `git commit -am "add features （｡>‿‿<｡ ）"`
- Push it to the fork you created: `git push -u origin some-branch-name`
- Submit a Pull Request and then cry! ｡･ﾟﾟ･(థ Д థ。)･ﾟﾟ･｡

## License
**Noelware Analytics for JVM** (`analytics-jvm`) is released under the **MIT License** by Noelware, with love. :3
