---
layout: post
title: Google Calendar api 에러 GoogleAuthException 
categories: Error
tags: 
 - Android Studio
 - Java
 - api
---


google calendar api 때문에 한 일주일 넘게 헤메었다. 

계정 선택창 이후에 권한동의 화면이 뜨지않고, 그상태로 권한이 필요한 작업을 진행하면 강제종료된다. 

아무래도 권한이 없어서 생긴 에러같다.  

하지만 debug.keystore 에서 얻은 지문으로 제대로 등록했고 api도 활성화한 상태였다. auth 동의 화면 설명을 다시 읽어보니...   

> ### 얼마나 많은 사용자가 내 앱을 사용할 수 있나요?
> ##### 게시 상태가 '테스트 중'인 앱은 테스트 사용자 목록에 있는 Google 계정의 승인을 요청할 수 있습니다.   

직접 테스트 계정을 추가해서 시도했더니 성공했다. 

내프로젝트 > auth 동의 화면 > 앱 등록 수정 > 테스트 사용자에서 추가하면 된다.  

앞으로 api 등록시에 안내문구를 잘 보아야겠다. 



로그캣 

```
I/Choreographer: Skipped 125 frames! The application may be doing too much work on its main thread.
D/EGL_emulation: eglMakeCurrent: 0xdca9f640: ver 2 0 (tinfo 0xe489b0f0)
I/chatty: uid=10100(com.example.myapplication) RenderThread identical 1 line
D/EGL_emulation: eglMakeCurrent: 0xdca9f640: ver 2 0 (tinfo 0xe489b0f0)
D/EGL_emulation: eglMakeCurrent: 0xdca9f640: ver 2 0 (tinfo 0xe489b0f0)
D/EGL_emulation: eglMakeCurrent: 0xdca9f640: ver 2 0 (tinfo 0xe489b0f0)
W/System.err: com.google.api.client.googleapis.extensions.android.gms.auth.GoogleAuthIOException
W/System.err: at com.google.api.client.googleapis.extensions.android.gms.auth.GoogleAccountCredential$RequestHandler.intercept(GoogleAccountCredential.java:301)
at com.google.api.client.http.HttpRequest.execute(HttpRequest.java:868)
at com.google.api.client.googleapis.services.AbstractGoogleClientRequest.executeUnparsed(AbstractGoogleClientRequest.java:419)
at com.google.api.client.googleapis.services.AbstractGoogleClientRequest.executeUnparsed(AbstractGoogleClientRequest.java:352)
W/System.err: at com.google.api.client.googleapis.services.AbstractGoogleClientRequest.execute(AbstractGoogleClientRequest.java:469)
at com.example.myapplication.MainActivity.getCalendarID(MainActivity.java:371)
at com.example.myapplication.MainActivity.access$800(MainActivity.java:53)
at com.example.myapplication.MainActivity$MakeRequestTask.createCalendar(MainActivity.java:484)
at com.example.myapplication.MainActivity$MakeRequestTask.doInBackground(MainActivity.java:434)
at com.example.myapplication.MainActivity$MakeRequestTask.doInBackground(MainActivity.java:397)
at android.os.AsyncTask$2.call(AsyncTask.java:333)
at java.util.concurrent.FutureTask.run(FutureTask.java:266)
at android.os.AsyncTask$SerialExecutor$1.run(AsyncTask.java:245)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1162)
W/System.err: at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:636)
at java.lang.Thread.run(Thread.java:764)
W/System.err: Caused by: com.google.android.gms.auth.GoogleAuthException: ServiceDisabled
W/System.err: at com.google.android.gms.auth.zze.zzb(Unknown Source:20)
at com.google.android.gms.auth.zzd.zza(Unknown Source:77)
at com.google.android.gms.auth.zzd.zzb(Unknown Source:20)
at com.google.android.gms.auth.zzd.getToken(Unknown Source:7)
at com.google.android.gms.auth.zzd.getToken(Unknown Source:5)
W/System.err: at com.google.android.gms.auth.zzd.getToken(Unknown Source:2)
at com.google.android.gms.auth.GoogleAuthUtil.getToken(Unknown Source:55)
W/System.err: at com.google.api.client.googleapis.extensions.android.gms.auth.GoogleAccountCredential.getToken(GoogleAccountCredential.java:269)
at com.google.api.client.googleapis.extensions.android.gms.auth.GoogleAccountCredential$RequestHandler.intercept(GoogleAccountCredential.java:294)
... 15 more
```






