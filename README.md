# aspectj-plugin
plugin for packaging function of message handler
## Introduce
Aspectj-Plugin has packaged code like
```groovy
variants.all { variant ->
            def javaCompile = variant.javaCompile
            javaCompile.doLast {
                String[] args = ["-showWeaveInfo",
                                 "-1.8",//the java version of module
                                 "-inpath", javaCompile.destinationDir.toString(),
                                 "-aspectpath", javaCompile.classpath.asPath,
                                 "-d", javaCompile.destinationDir.toString(),
                                 "-classpath", javaCompile.classpath.asPath,
                                 "-bootclasspath", project.android.bootClasspath.join(File.pathSeparator)]
                log.debug "ajc args: " + Arrays.toString(args)

                MessageHandler handler = new MessageHandler(true);
                new Main().run(args, handler)
                for (IMessage message : handler.getMessages(null, true)) {
                    switch (message.getKind()) {
                        case IMessage.ABORT:
                        case IMessage.ERROR:
                        case IMessage.FAIL:
                            log.error message.message, message.thrown
                            break
                        case IMessage.WARNING:
                            log.warn message.message, message.thrown
                            break
                        case IMessage.INFO:
                            log.info message.message, message.thrown
                            break
                        case IMessage.DEBUG:
                            log.debug message.message, message.thrown
                            break
                    }
                }
            }
        }
```
So this plugin can save the time when you deploy the AOP program technology of Aspectj.
That was the meaning of aspectj-plugin existence.
**And take consideration of difference of application and library, plugin has judged the type of variant, so you can use it anywhere (module of application or library)**
## How to use
First, you should clone this repsoitory and put the directory in your android project, and then config the aspectj-plugin repsoitory in your project's root **build.gradle** or **settings.gradle**
```groovy
repsoitory{
  maven{ url uri("$rootDir/aspectj-plugin")} //or locate to "https://github.com/devmeng/aspectj-plugin/tree/master"
}
```
After that, I wish you to add classpath under your project's root **build.gradle**, code like
```groovy
buildscript{
     dependencies{
            ...
            classpath "com.devmeng.plugin:aspectj-plugin:1.0.0"
     }
}

```
Well, after configs aspectj-plugin repsoitory, we will apply plugin under build.gradle of module, like
```groovy
apply plugin: "aspectj" //or in action scope of plugins, like: id 'aspectj'
```
## Best Wishes
Ok, that what should we do!
Enjoy!:)
