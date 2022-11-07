# aspectj-plugin
plugin for aspectj
## Introduce
Aspectj-Plugin has packaged code like
```groovy
variants.all { variant ->
            def javaCompile = variant.javaCompile
            javaCompile.doLast {
                String[] args = ["-showWeaveInfo",
                                 "-1.8",//对应插件module声明的Java版本
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
**Takes full account of difference of application and library, plugin has judged the type of variant, so you can use it anywhere (module of application or library)**
## How to use
clone this repsoitory and put the directory in your android project, and then config the aspectj-plugin repsoitory in your root project build.gradle or settings.gradle
```groovy
repsoitory{
  maven{ url uri("../YourProjectName/aspectj-plugin")}
}
```
Well, after configs aspectj-plugin repsoitory, we will apply plugin, code like
```groovy
apply plugin: "aspectj"
```
## Best Wishes
Ok, that what should we do!
Enjoy!:)
