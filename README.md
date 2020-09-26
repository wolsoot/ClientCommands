<img src="icon.png" align="right" width="180px"/>

# Cotton Client Commands

[![Maven metadata URL](https://img.shields.io/maven-metadata/v/https/server.bbkr.space:8081/artifactory/libs-release/io/github/cottonmc/cotton-client-commands/maven-metadata.xml.svg)](https://server.bbkr.space:8081/artifactory/libs-release/io/github/cottonmc/cotton-client-commands)

[>> Downloads <<](https://github.com/CottonMC/ClientCommands/releases)

A Minecraft library for 1.15 (updated to work with 1.16.3) that adds support for client-side commands.
The main point of this fork is for publishing the ClientCommands packages to GitHub (the packages published by the upstream repo were inaccessible at the time of creating this fork).

**This mod is open source and under a permissive license.** As such, it can be included in any modpack on any platform without prior permission. We appreciate hearing about people using our mods, but you do not need to ask to use them. See the [LICENSE file](LICENSE) for more details.

## Usage
Note that unfortunately GitHub Packages requires you to authenticate with GitHub in order to install a package.
You can create a personal access token at https://github.com/settings/tokens. The "read:packages" scope should be sufficient.

Add a dependency in your `build.gradle` or `build.gradle.kts`:

```groovy
repositories {
    maven {
		name = "GitHubPackages"
		url = uri("https://maven.pkg.github.com/wolsoot/ClientCommands")
		credentials {
			username = githubPackagesUser ?: System.getenv("USERNAME")
			password = githubPackagesToken ?: System.getenv("TOKEN")
		}
	}
}

dependencies {
    modImplementation "io.github.wolsoot:cotton-client-commands:<latest version>"
}
```

<details>
    <summary>build.gradle.kts</summary><p>
    
```kotlin
repositories {
    maven {
		name = "GitHubPackages"
		url = uri("https://maven.pkg.github.com/wolsoot/ClientCommands")
		credentials {
			username = githubPackagesUser ?: System.getenv("USERNAME")
			password = githubPackagesToken ?: System.getenv("TOKEN")
		}
	}
}

dependencies {
    modImplementation("io.github.wolsoot:cotton-client-commands:<latest version>")
}
```
</details>

Register the commands with a `ClientCommandPlugin`:

```java
import com.mojang.brigadier.CommandDispatcher;
import io.github.cottonmc.clientcommands.*;
import net.minecraft.server.command.CommandSource;
import net.minecraft.text.LiteralText;

public class MyCommands implements ClientCommandPlugin {
    @Override
    public void registerCommands(CommandDispatcher<CottonClientCommandSource> dispatcher) {
        dispatcher.register(ArgumentBuilders.literal("client-commands").executes(
            source -> {
                source.getSource().sendFeedback(new LiteralText("Hello, world!"));
                return 1;
            }
        ));
    }
}
```

And finally, add the dependency and register the plugin entrypoint in your `fabric.mod.json`:

```json
{
  "depends": {
    "cotton-client-commands": "^1.0.1"
  },
  "entrypoints": {
    "cotton-client-commands": ["path.to.MyCommands"]
  }
}
```

The classes `ArgumentBuilders` and `CottonClientCommandSource` are provided as
alternatives to `CommandManager` and `ServerCommandSource`. 
