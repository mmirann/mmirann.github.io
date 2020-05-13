---
layout: post
title: "Update blog-3"
subtitle: "블로그 업데이트 일지"
categories: [Etc/Github Blog]
tags: [Github, Blog]
comments: true
---

> 블로그 업데이트 - 리뉴얼

이전 테마를 버리고 새로 싹 갈아엎었다. 이전 테마에서 마음에 안들었던 건 Width를 수정했음에도 불구하고 사이드바떄문에 화면이 좁아보여서 답답했다. 그리고 Customizing에 약간씩 제약이 있었다. 완전히 바꾸지를 못해서 새로 바꿀 테마는 포스팅이 화면 전체를 차지하고, 필요한 기능만 있으면서 Customzing에 제약이 없는 테마로 선택했다. 출처는 [Type-theme](https://github.com/rohanchandra/type-theme).

<center><point> Before-> After </point></center>
<br>
![](/assets/img/post/github1.PNG)

![](/assets/img/post/github2.PNG)

정말 미니멀한 테마여서 이것저것 추가하기가 수월했다! 추가한 기능들을 간단히 적어보고자 한다.


> <point>카테고리 추가</point>

기존테마에는 카테고리가 존재하지 않는다. 그래서 추가했다. [참고링크](https://devyurim.github.io/development%20environment/github%20blog/2018/08/07/blog-6.html) 구글링하다가 같은 테마를 사용중이신 분이 카테고리 만드는 법을 잘 정리해놓으셔서 참고했다. 포스트도 폴더별로 정리할 수 있어서 너무너무 도움이 됐다. 다만 모바일에서 볼 땐 카테고리가 잘려서 코드를 좀 더 수정해서 완성했다. 사실 원래 햄버거로 만드려고 했으나 급 귀찮아져서 그냥 media를 이용해서 Float랑 Position만 수정했다! 

> <point>모바일 최적화</point>

이전 테마는 기능이 많은 만큼 손대지 않아도 되는 것들이 많았다. 특히 모바일 최적화가 정말 잘되어있었는데 이 테마는 거의.. 안되어있다. 그래서 모바일로 블로그를 볼 때면 돋보기로 확대한 것마냥 글씨가 어마무시하게 커지고 Padding도 조절이 안됐다. 자세한 수정 목록은 [이커밋](https://github.com/mmirann/mmirann.github.io/commit/4f8e195c36db4436f61d1d160105a09ee09bbc3e)을 참고!

> <point>Search, Tag, Disqus </point>

이 기능들은 config.yml에서 수정하면 된다. Search를 TRUE로 설정하면 Search바가 header에 나타난다. Tag도 Tag.html 파일에서 hide를 false로 설정하면 header에 나타난다. 기존엔 About도 있었지만, 지저분해지는 게 싫어서 About은 메뉴로 뺐다. 그리고 Disqus는 Disqus 홈페이지에 가서 웹페이지를 등록하고 shortname도 등록한 뒤에 config.yml에 등록하면 된다. 

> <point>코드 블록, 폰트 </point>

재차말하지만 정말 미니멀한 테마여서 코드 블록도 설정이 안되어있다..정말 태초의 상태다. 가독성 구리다는 뜻. 그래서 그냥 이전 테마에서 code, pre 부분을 긁어왔다. 아주아주 간단하게 적용 끝! 폰트는 역시나 나눔고딕. 맥북이 도착하면 애플 기본폰트를 우선으로 적용하게 바꿔야겠다. 폰트랑 폰트색상은 _reset.scss에서 수정하면 된다. [이커밋](https://github.com/mmirann/mmirann.github.io/commit/4cab3e0db13f594e1bc9deac59c7ad39f737db83)에 정리되어있다.

---
이제 정말 군더더기 없이 마음에 쏙 드는 블로그가 완성됐다.ㅋㅋ 더이상 추가하고 싶은 것도, 수정하고 싶은 것도 없어서 글을 더 쓸 수 있을까? 쨌든 덕분에 오랜만에 HTML, CSS, JS 공부했다. 