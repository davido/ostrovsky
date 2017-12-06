# Reproducer for https://github.com/google/auto/issues/560 

To reproduce the problem on Java 9, on Bazel, build Bazel@HEAD with
this patch included:
https://github.com/hhclam/bazel/commit/64212c8026df9fc6361d5af8414acc373e221955
and execute:

```
$ ~/projects/bazel/bazel-bin/src/bazel build :breakage --host_java_toolchain=@bazel_tools//tools/jdk:jdk9 --java_toolchain=@bazel_tools//tools/jdk:jdk9
INFO: Analysed target //:breakage (0 packages loaded).
INFO: Found 1 target...
ERROR: /home/davido/projects/ostrovsky/BUILD:1:1: Building libbreakage.jar (2 source files) and running annotation processors (AutoAnnotationProcessor, AutoValueProcessor) failed (Exit 1)
src/main/java/org/ostrovsky/auto/breakage/Commands.java:33: error: @AutoAnnotation processor threw an exception: java.lang.NullPointerException
  public static CommandName named(String value) {
                            ^
  	at com.google.auto.value.processor.AutoAnnotationProcessor.getTypeMirror(AutoAnnotationProcessor.java:470)
  	at com.google.auto.value.processor.AutoAnnotationProcessor.getReferencedTypes(AutoAnnotationProcessor.java:450)
  	at com.google.auto.value.processor.AutoAnnotationProcessor.processMethod(AutoAnnotationProcessor.java:152)
  	at com.google.auto.value.processor.AutoAnnotationProcessor.process(AutoAnnotationProcessor.java:129)
  	at com.google.auto.value.processor.AutoAnnotationProcessor.process(AutoAnnotationProcessor.java:113)
  	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.callProcessor(JavacProcessingEnvironment.java:968)
  	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.discoverAndRunProcs(JavacProcessingEnvironment.java:884)
  	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.access$2200(JavacProcessingEnvironment.java:108)
  	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment$Round.run(JavacProcessingEnvironment.java:1206)
  	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.doProcessing(JavacProcessingEnvironment.java:1315)
  	at jdk.compiler/com.sun.tools.javac.main.JavaCompiler.processAnnotations(JavaCompiler.java:1246)
  	at jdk.compiler/com.sun.tools.javac.main.JavaCompiler.compile(JavaCompiler.java:922)
  	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.lambda$doCall$0(JavacTaskImpl.java:100)
  	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.handleExceptions(JavacTaskImpl.java:142)
  	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.doCall(JavacTaskImpl.java:96)
  	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.call(JavacTaskImpl.java:90)
  	at com.google.devtools.build.buildjar.javac.BlazeJavacMain.compile(BlazeJavacMain.java:107)
  	at com.google.devtools.build.buildjar.SimpleJavaLibraryBuilder$1.invokeJavac(SimpleJavaLibraryBuilder.java:105)
  	at com.google.devtools.build.buildjar.ReducedClasspathJavaLibraryBuilder.compileSources(ReducedClasspathJavaLibraryBuilder.java:54)
  	at com.google.devtools.build.buildjar.SimpleJavaLibraryBuilder.compileJavaLibrary(SimpleJavaLibraryBuilder.java:108)
  	at com.google.devtools.build.buildjar.SimpleJavaLibraryBuilder.run(SimpleJavaLibraryBuilder.java:116)
  	at com.google.devtools.build.buildjar.BazelJavaBuilder.processRequest(BazelJavaBuilder.java:105)
  	at com.google.devtools.build.buildjar.BazelJavaBuilder.runPersistentWorker(BazelJavaBuilder.java:67)
  	at com.google.devtools.build.buildjar.BazelJavaBuilder.main(BazelJavaBuilder.java:45)
java.lang.RuntimeException: java.lang.NullPointerException
	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.handleExceptions(JavacTaskImpl.java:158)
	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.doCall(JavacTaskImpl.java:96)
	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.call(JavacTaskImpl.java:90)
	at com.google.devtools.build.buildjar.javac.BlazeJavacMain.compile(BlazeJavacMain.java:107)
	at com.google.devtools.build.buildjar.SimpleJavaLibraryBuilder$1.invokeJavac(SimpleJavaLibraryBuilder.java:105)
	at com.google.devtools.build.buildjar.ReducedClasspathJavaLibraryBuilder.compileSources(ReducedClasspathJavaLibraryBuilder.java:54)
	at com.google.devtools.build.buildjar.SimpleJavaLibraryBuilder.compileJavaLibrary(SimpleJavaLibraryBuilder.java:108)
	at com.google.devtools.build.buildjar.SimpleJavaLibraryBuilder.run(SimpleJavaLibraryBuilder.java:116)
	at com.google.devtools.build.buildjar.BazelJavaBuilder.processRequest(BazelJavaBuilder.java:105)
	at com.google.devtools.build.buildjar.BazelJavaBuilder.runPersistentWorker(BazelJavaBuilder.java:67)
	at com.google.devtools.build.buildjar.BazelJavaBuilder.main(BazelJavaBuilder.java:45)
Caused by: java.lang.NullPointerException
	at com.google.auto.value.processor.AutoAnnotationProcessor.getTypeMirror(AutoAnnotationProcessor.java:470)
	at com.google.auto.value.processor.AutoAnnotationProcessor.getReferencedTypes(AutoAnnotationProcessor.java:450)
	at com.google.auto.value.processor.AutoAnnotationProcessor.processMethod(AutoAnnotationProcessor.java:152)
	at com.google.auto.value.processor.AutoAnnotationProcessor.process(AutoAnnotationProcessor.java:129)
	at com.google.auto.value.processor.AutoAnnotationProcessor.process(AutoAnnotationProcessor.java:113)
	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.callProcessor(JavacProcessingEnvironment.java:968)
	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.discoverAndRunProcs(JavacProcessingEnvironment.java:884)
	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.access$2200(JavacProcessingEnvironment.java:108)
	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment$Round.run(JavacProcessingEnvironment.java:1206)
	at jdk.compiler/com.sun.tools.javac.processing.JavacProcessingEnvironment.doProcessing(JavacProcessingEnvironment.java:1315)
	at jdk.compiler/com.sun.tools.javac.main.JavaCompiler.processAnnotations(JavaCompiler.java:1246)
	at jdk.compiler/com.sun.tools.javac.main.JavaCompiler.compile(JavaCompiler.java:922)
	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.lambda$doCall$0(JavacTaskImpl.java:100)
	at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.handleExceptions(JavacTaskImpl.java:142)
	... 10 more
Target //:breakage failed to build
```

Note that all iss fine on Maven, compiler-plugin 3.7.0:

```
$ mvn package
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building ostrovsky 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ ostrovsky ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /home/davido/projects/ostrovsky/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:compile (default-compile) @ ostrovsky ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 2 source files to /home/davido/projects/ostrovsky/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ ostrovsky ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /home/davido/projects/ostrovsky/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:testCompile (default-testCompile) @ ostrovsky ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ ostrovsky ---
[INFO] No tests to run.
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ ostrovsky ---
[INFO] Building jar: /home/davido/projects/ostrovsky/target/ostrovsky-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.945 s
[INFO] Finished at: 2017-12-06T08:36:20+01:00
[INFO] Final Memory: 19M/64M
[INFO] ------------------------------------------------------------------------
```
