---
layout: post
title: Android Fragment 1
description: >
  Android Fragment 생성 및 commit
image: 
noindex: true
---

지난 포스팅에서 하던 MVP + Clean Architecture 구조를 잡다가,
삼천포로 빠져서 Fragment 만들고 commit하는데 시간을 다 써버렸다.
기왕 이렇게 된거 Fragment에 대해 이야기해보자.

Fragment의 생성: Factory Method Pattern
---------------------------------------
> Fragment를 생성하는 Class와 생성되는 Fragment 간의 의존성을 줄이기 위하여,
> Factory Method Pattern을 이용하였다.

- 생성자는 Private
```java
private MainContentFragment() { }
```


- public static method인 newInstance를 작성
```java
public static MainContentFragment newInstance(String param) {
	Bundle args = new Bundle();
	args.putString("custom_bundle_key_for_string", param);

	MainContentFragment fragment = new MainContentFragment();
	fragment.setArguments(args);

	return fragment;
}
```


* newInstance()와 Bundle을 이용하는 이유
  - [참고: Android Fragment newInstance()로 생성하는 이유][link-ref1]


Fragment를 화면에 나타내기
-------------------------
> Fragment를 화면에 나타내는 방법은 하나의 Method로 만들어서 사용하는 것이 좋겠다.
> 자주 쓸것이므로.


- FragmentManager 객체를 얻어와 그 Method를 이용한다.
  - 사용하는 Activity가 Activity를 상속 받았다면, getFragmentManager()를 이용
  - 사용하는 Activity가 AppCompatActivity를 상속 받았다면, getSupportedFragmentManager()를 이용
```java
private void commitFragment(/* Fragment를 배치할 Layout resource id */ int layoutResId,
							/* 배치할 Fragment */ Fragment fragment) {
	getSupportFragmentManager()
			.beginTransaction() // FragmentTransaction 객체를 반환 (Fragment 나타낼 때마다 call)
			.setTransition(FragmentTransaction.TRANSIT_FRAGMENT_OPEN) // 전환 효과
			.addToBackStack(fragment.getClass().getName()/* name */) // 현재 Fragment의 위에 쌓고 싶다면
			.replace(layoutResId, fragment)
			.commit();
}
```


- addToBackStack()에 대한 설명
  - [참고: 안드로이드 Fragment BackStack에 대한 고찰][link-ref2]


> 다음 Fragment 관련 포스팅은 Fragment 생성하는 부분을 분리하여 하나의 Class로 만들 것이다.
> Fragment간 직접적인 참조를 줄여보고자 (구조를 아름답게 가져가보고자) 하려고 하는데,
> 어떤 방향으로 가는 것이 좋을지 고민해보아야겠다.


참고 사이트
-----------
* [Android Fragment newInstance()로 생성하는 이유][link-ref1]
* [안드로이드 Fragment BackStack에 대한 고찰][link-ref2]

[link-ref1]: https://m.blog.naver.com/tpgns8488/220989078813
[link-ref2]: https://soulduse.tistory.com/23