# Env test
When using `ConfigMapping` all environment variables on the build machine starting with the prefix, that is not included in the conf, will be included in the build and required to be present when running the app.

## Reproduce
```bash
export PREFIX_NEW=hello && ./mvnw clean package && unset PREFIX_NEW && java -jar target/quarkus-app/quarkus-run.jar
```

The following error is raised:

```bash
Exception in thread "main" java.lang.reflect.InvocationTargetException
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:568)
	at io.quarkus.bootstrap.runner.QuarkusEntryPoint.doRun(QuarkusEntryPoint.java:61)
	at io.quarkus.bootstrap.runner.QuarkusEntryPoint.main(QuarkusEntryPoint.java:32)
Caused by: java.lang.ExceptionInInitializerError
	at io.quarkus.runner.ApplicationImpl.<clinit>(Unknown Source)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:77)
	at java.base/jdk.internal.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:499)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:480)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:70)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:44)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:124)
	at io.quarkus.runner.GeneratedMain.main(Unknown Source)
	... 6 more
Caused by: io.smallrye.config.ConfigValidationException: Configuration validation failed:
	prefix.new does not map to any root
	at io.smallrye.config.ConfigMappingProvider.mapConfigurationInternal(ConfigMappingProvider.java:979)
	at io.smallrye.config.ConfigMappingProvider.lambda$mapConfiguration$3(ConfigMappingProvider.java:935)
	at io.smallrye.config.SecretKeys.doUnlocked(SecretKeys.java:28)
	at io.smallrye.config.ConfigMappingProvider.mapConfiguration(ConfigMappingProvider.java:935)
	at io.smallrye.config.ConfigMappings.mapConfiguration(ConfigMappings.java:91)
	at io.smallrye.config.SmallRyeConfigBuilder.build(SmallRyeConfigBuilder.java:624)
	at io.quarkus.runtime.generated.Config.<clinit>(Unknown Source)
	... 16 more
```