# gradle-aspectj-binary

[![Build Status](https://travis-ci.org/sedovalx/gradle-aspectj-binary.svg?branch=master)](https://travis-ci.org/sedovalx/gradle-aspectj-binary)
[ ![Download](https://api.bintray.com/packages/sedovalx/com.github.sedovalx/com.github.sedovalx.gradle-aspectj-binary/images/download.svg) ](https://bintray.com/sedovalx/com.github.sedovalx/com.github.sedovalx.gradle-aspectj-binary/_latestVersion)

Gradle plugin for AspectJ binary weaving. It works similar to https://github.com/jcabi/jcabi-maven-plugin 
doing the weaving on the weaving over already compiled classes. It is particularly helpful if the source code
  is written in a language different from Java. For example, [Kotlin](https://kotlinlang.org), as it produces 
  fully compatible Java bytecode. As far as I know there was a problem with Kotlin inlining in AspectJ 1.8.9, that 
   was fixed in the 1.8.10, so the later is used.
   
> It goes without saying that the plugin works for Java sources as well.   
  
### Usage

  Here is an example project in the `examples` folder. Basically you need to add the plugin to your build script
  
      buildscript {
        repositories {
          jcenter()
        }
        dependencies {
          classpath "com.github.sedovalx.gradle:gradle-aspectj-binary:$pluginVersion"
        }
      }
      
      apply plugin: 'com.github.sedovalx.gradle-aspectj-binary'
      
  `weaveClasses` task becomes available after that. It makes sense to add it as a dependency for the `classes` task
  
      weaveClasses.dependsOn compileJava
      classes.dependsOn weaveClasses
  
  so you can just run the build and have all your aspects in the `main` source set applied.
  
  > You need to weave both aspects and classes where aspects should be applied. So if you have aspect 
  classes in a project A and classes to be weaved in a project B you should add the `weaveClasses` task to the build 
  process of both projects.
  
### Available configuration
  
  For now the `weaceClasses` task can be configured with two parameters
  
      weaveClasses {
        source = '1.7'  // default value
        target = '1.7'  // default value
      }
      
  Provided values are passed as it is to the [ajc](http://www.eclipse.org/aspectj/doc/next/devguide/ajc-ref.html) compiler parameters.   

  
