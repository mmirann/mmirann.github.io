---
layout: post
title: "Update blog-2"
subtitle: "블로그 업데이트 일지"
categories: devlog
tags: blog
comments: true
---

> 블로그 업데이트 - 검색 엔진 등록, 코딩 폰트, 전체 Width, 코딩 블록

`2020-04-29` 수정 목록
- [검색 엔진 등록](#%ea%b2%80%ec%83%89-%ec%97%94%ec%a7%84-%eb%93%b1%eb%a1%9d)
- [코딩 폰트](#%ec%bd%94%eb%94%a9-%ed%8f%b0%ed%8a%b8)
- [전체 Width](#%ec%a0%84%ec%b2%b4-width)
- [코딩 블록](#%ec%bd%94%eb%94%a9-%eb%b8%94%eb%a1%9d)

대박 하라는 공부는 안하고 블로그 꾸미기만 겁나 하는 중;; HTML 마스터할 것 같다. 이제 수정 안해..!^\_^

## 검색 엔진 등록

별다른 조치없이 그냥 글쓰기만 해도 검색에 노출되었던 네이버 블로그와 달리, 깃허브 블로그는 내가 직접! 검색 엔진마다 등록해야한다. 등록을 안하자니 이건 일기장이랑 다름이 없다. 그래서 욕심 부리지 않고 [Google search console](https://search.google.com/search-console/about)와 [Naver search advisor](https://searchadvisor.naver.com/)에만 등록을 해놨다. 구글 서치 콘솔은 노출 확인을 했고 네이버는 오늘해서 확인이 아직 안된다. 확인 절차는 둘이 비슷하다. 구글과 네이버 둘다 부여받은 html을 root에 업로드하면 본인 사이트인지 확인이 되고, 구글은 sitemap.xml을 업로드, 네이버는 feed.xml과 sitemap.xml을 업로드하면 된다.

## 코딩 폰트

코딩 폰트를 바꿨다. 원래는 나눔 고딕 코딩으로 하려고 했는데 왜인지 모르겠지만 적용이 안된다. [Google fonts](https://fonts.google.com/specimen/Nanum+Gothic+Coding) 여기 있는 Font Familys 그대로 복사해도 안된다. 그래서 깔끔하게 버리고 심플 of 심플인 Consolas로 컴백! 그리고 한글 폰트를 지금도 나눔고딕이랑 맑은 고딕 중에 고민이다. 본문 폰트는 나눔고딕이 깔끔한데 주석으로 나눔고딕이 들어가니까 넘 별로.

## 전체 Width

기존에는 PC로 블로그를 볼 때 오른쪽에 여백이 많이 보였다. 나는 화면에 꽉 차보이길 원했으므로 F12를 누르고 열심히 테마 탐구를 했다. `content-inline.scss`에 들어가서 Max-width를 건들여서 수정했다! 훨씬 마음에 든다. 모르고 break-point를 수정해서 동적 길이가 안먹어 당황했지만 커밋목록에 들어가서 원래대로 수정했더니 해결했다! 커밋의 중요성

## 코딩 블록

여기서 의미하는 코딩블록은 `이 코딩블록`을 말한다. 마크다운은 다 좋은데 색상을 설정하려면 HTML로 설정을 해야한다. 번거롭다. 그나마 강조효과를 줄 수 있는 게 이 `코딩 블록`인데 처음엔 코딩 폰트를 따라가서 기본 서체인 굴림체가 출력되어 넘 구렸다. 그래서 위의 코딩 서체를 변경함과 동시에 size랑 color도 테마에 맞게 바꿔주었다-! `hydejack.css의 markdown-body code`를 수정하면 된다.

---

이제 남은 건 Search 기능과 ToC 기능이다ㅎ..
