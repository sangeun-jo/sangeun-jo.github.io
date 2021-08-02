--- 
layout: post
title: react js 푸시알림 flask 서버 연동하기
categories: Code
tags:
 - React Native
 - firebase
 - flask
---


react js와 firebase 푸시알림 조합은 많은데, flask 서버로 제어하는 건 정리된 게 없어서 올린다. 

우선 reactjs에서 firebase를 설치한다. 

react js App.js 측 코드 
```
import {React, useEffect} from 'react';
import firebase from 'firebase';

//firebase 프로젝트에서 확인 
const App = () => {
  const config = {
    apiKey: "",
    authDomain: "",
    projectId: "",
    storageBucket: "",
    messagingSenderId: "",
    appId: "",
    measurementId: ""
  };

  firebase.initializeApp(config);
  const messaging = firebase.messaging();

  //포그라운드 알림
  messaging.onMessage(function(payload){
    console.log(payload.data.title);
    console.log(payload.data.body);
  })

  //서비스 워커 등록
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker
        .register('firebase-messaging-sw.js')
        .then(function(registration) {
            console.log('[SW]: SCOPE: ', registration.scope);
            return registration.scope;
        })
        .catch(function(err) {
            return err;
        });
  }

  useEffect(() => {
    firebase.messaging().getToken()
    .then((deviceToken) => {
      console.log(deviceToken)      // save this token somewhere   
    })        
    .catch((err) => {console.log(err)})
  },[])

  return (
    <div>"test"</div>
  ); 
}

export default App;
```


public 폴더 아래에 firebase-messaging-sw.js 파일을 생성하여 서비스워커를 등록한다. 
```
//시점에 맞는 최신버전으로 임포트 할 것 
importScripts('https://www.gstatic.com/firebasejs/8.8.0/firebase-app.js') 
importScripts('https://www.gstatic.com/firebasejs/8.8.0/firebase-messaging.js')

const App = () => {
  const config = {
    apiKey: "",
    authDomain: "",
    projectId: "",
    storageBucket: "",
    messagingSenderId: "",
    appId: "",
    measurementId: ""
  };

firebase.initializeApp(config);
const messaging = firebase.messaging();

//백그라운드 알림 
messaging.setBackgroundMessageHandler(function (payload) {
    console.log('[firebase-messaging-sw.js] Received background message ', payload)
    const notificationTitle = payload.data.title
    const notificationOptions = {
        body: payload.data.body,
    }
    
    return self.registration.showNotification(notificationTitle, notificationOptions)
}); 

//클릭 이벤트 처리
self.addEventListener('notificationclick', (event) => {
    console.log(event)
    return event
});
```

이제 reactjs쪽 코드는 끝이 났다. 


flask 서버에서 처리를 해보자. 

우선 pyfcm, firebase_admin 모듈을 설치한다. 
firebase에서 새프로젝트를 생성하고, 웹 플랫폼을 추가한다. 프로젝트 설정페이지에서 서버키를 확인하고, json 형식의 SDK 구성 파일(google-services.json)을 다운로드 받는다. 


push.py 
```
import os
import json
from pyfcm import FCMNotification
import firebase_admin
from firebase_admin import messaging 
from firebase_admin import credentials

#푸시 알림 설정 
def set_admin():
	cred_path = os.path.join(os.getcwd(), "serviceAccountKey.json") //sdk 파일 경로 
	print(cred_path)
	cred = credentials.Certificate(cred_path)
	firebase_admin.initialize_app(cred)

#알림 전송 
def push_alert(token, title, body):
	registration_id = token 	

	push_service = FCMNotification("서버키")
	message_title = "face detected!"
	message_body = "detect time: "

	data_message = {}
	data_message["title"] = title 
	data_message["body"] = body

	json.dumps(data_message)
	result = push_service.notify_single_device(registration_id=registration_id, data_message=data_message)

	print(result)
```

사용예시

```
from push import push_alert, set_admin
...
push_alert("디바이스 기기 토큰", "title", "body") //기기 토큰은 react js 서버를 실행하면 console 창에서 볼 수 있다. 
```

flask 서버에서 post 방식으로 token과 제목, 내용을 react 서버에 넘겨주면, react 서버에서 알림을 발생한다.  



