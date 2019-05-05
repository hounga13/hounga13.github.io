---
layout: post
title: MVP + Clean Architecture 구조 잡기 1
description: >
  MVP + Clean Architecture 구조 잡기 1
image: 
noindex: true
---


### 무슨 구조를 사용할까?

> 소프트웨어 아키텍쳐에 대해 문외한이지만,
> 회사에서 안드로이드 업무를 시작하고 맡은 프로젝트에서 사용한
> MVP 패턴과 **Clean architecture** 의 조합을 사용해보기로 하였다.


### MVP + Clean Architecture

> 안드로이드는 구글에 검색하면 자료가 참 많이 나와서 좋다.
> [우아한형제들 기술 블로그][link-wooatechblog]와 Google의 [MVP + Clean Architecture google sample app][link-googlesampleapp] github를 참고하였다.

- entity, data, domain, presentation으로 패키지를 나누어주고,


- 아직 비즈니스 모델?의 방향이 정해지지 않아서 presentation의 구조부터 잡아보기로 하였다.


- Presentation의 근간이 되는 BasePresenter와 BaseView를 구현
```java
public interface BaseView<T extends BasePresenter> {
	void setPresenter(T presenter);
}
```
```java
public interface BasePresenter {
	void start();

	void resume();

	void pause();

	void stop();

	void destroy();
}
```


- Contract를 하나 만들어서 MainContentPresenter와 MainContentFragment를 연결해 줘야지
```java
public interface MainContentContract {
	public interface Presenter extends BasePresenter {
		//TODO: View에서 호출할 수 있는 method 구현
	}

	public interface View extends BaseView<Presenter> {
		//TODO: Presenter에서 호출할 수 있는 method 구현
	}
}
```


- MainContentFragment가 MainContentContract.View를 implementation
```java
public class MainContentFragment extends Fragment implements MainContentContract.View {
	//TODO: Override method
}
```


- MainContentPresenter가 MainContentContract.Presenter를 implementation
```java
public class MainContentPresenter implements MainContentContract.Presenter {
	//TODO: Override method
}
```


> 일단 이렇게 구조를 만들어 두면, 이후에는 Ctrl + C, Ctrl + V의 향연이 될 것이다. Good!

### 참고 사이트

* [우아한형제들 기술 블로그][link-wooatechblog]
* [MVP + Clean Architecture google sample app][link-googlesampleapp]


[link-wooatechblog]: http://woowabros.github.io/experience/2019/01/17/baeminapp-clean-architecture.html
[link-googlesampleapp]: https://github.com/googlesamples/android-architecture/tree/todo-mvp-clean/