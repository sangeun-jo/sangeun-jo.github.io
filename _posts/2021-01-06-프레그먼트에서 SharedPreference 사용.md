--- 
layout: post
title: 프레그먼트에서 SharedPreference 사용
categories: Java
tags:
 - Android studio
 - fragment 
 - getSharedPreferences
---



프레그먼트에서 getSharedPreferences 메서드를 사용하면 에러가 난다. 

getSharedPreferences는 액티비티 메서드이므로, 프레그먼트의 부모 액티비티를 얻어와서 사용하면 된다.  
```
name = getActivity().getSharedPreferences("name", getActivity().MODE_PRIVATE); 
```

* 참고

프레그먼트는 액티비티 메서드나 변수가 필요할 때, getActivity()를 통해 부모 액티비티를 얻어올 수 있다. 








