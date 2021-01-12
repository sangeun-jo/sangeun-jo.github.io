--- 
layout: post
title: spring-boot 서버 시작 시 에러
categories: Java
tags:
 - Java
 - Maven 
 - String boot
 
---


spring-boot 프로젝트를 생성하고 ```mvnw spring-boot:run`` 명령어를 입력 하자마자 에러가 났다. 

에러메시지를 살펴보니 
 ```
Description:

Web server failed to start. Port 8080 was already in use.

Action:

Identify and stop the process that's listening on port 8080 or configure this application to listen on another port.

[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  21.233 s
[INFO] Finished at: 2021-01-12T21:06:44+09:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.springframework.boot:spring-boot-maven-plugin:2.4.1:run (default-cli) on project demo: Application finished with exit code: 1 -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException

```
이클립스에서 다른 프로젝트를 하다가 8080 포트가 켜져있어서 발생한 오류였다. 

이클립스에서 돌아가는 톰캣을 중지시키고 다시 시작하니 해결되었다. 


