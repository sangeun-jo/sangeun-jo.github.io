---
layout: post
title: IntelliyJ 에서 gradle로 querydsl 연동하기 
categories: Java
tags: 
 - gradle project
 - intelliry J 
 - querydsl
---


gradle 버전이 달라지면서 querydsl을 연동하는 방식이 달라졌다. 책을 보고 따라하다가 안되서 구글링해보면서 방법을 찾았다.

```
//gradle version 6.7.1
plugins {
    id 'org.springframework.boot' version '1.5.8.RELEASE'
    id 'io.spring.dependency-management' version '1.0.9.RELEASE'
    id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
    id 'java'

}

sourceCompatibility = 1.8

ext{
    springBootVersion='1.5.8.RELEASE'
    querydslVersion = "4.1.4"
}

repositories {
    mavenCentral()
}


dependencies {
    compile('org.springframework.boot:spring-boot-starter-data-jpa')
    compile group: 'org.hsqldb', name: 'hsqldb', version:'2.3.2'
    compile  "com.querydsl:querydsl-core:${querydslVersion}"
    compile  "com.querydsl:querydsl-jpa:${querydslVersion}"
    compile  "com.querydsl:querydsl-apt:${querydslVersion}"
  
}


def querydslDir = file('src/main/java')

querydsl {
    jpa = true
    querydslSourcesDir = querydslDir
}

sourceSets {
    main.java.srcDir querydslDir
}

configurations {
    querydsl.extendsFrom compileClasspath
}

compileQuerydsl {
    options.annotationProcessorPath = configurations.querydsl
}
```
