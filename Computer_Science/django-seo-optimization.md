# How to SEO on django project

SEO is one of the most important feature when talking about the quality of a website. In django, there are many packages that enables django to easily manage SEO configurations.

## Configure meta tag in template
reference: [http://ogp.me/](http://ogp.me/), [https://moz.com/learn/seo/meta-description](https://moz.com/learn/seo/meta-description)

The meta tag works to show the search results' description and facebook's description. Normal meta tag is usually deals for google's search engine and opengraph is for facebook.

```
<meta name="keywords" content="원룸, 월세방, 월세, 맞춤형, 부동산, 투룸, 오피스텔">
  <meta name="description" content="더이상 발품팔지 마세요, 원하시는 방을 대신 찾아드립니다. 원하는 조건을 입력하고 제시된 매물을 온라인에서 살펴보세요!"">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
          
  <meta property="og:title" content="달팽이등껍질 | 쉽고 편하게 찾는 맞춤형 원룸구하기">
        <meta property="og:type" content="website">
        <meta property="og:url" content="https://snailshell.co">
        <meta property="og:site_name" content="달팽이등껍질">
        <meta property="og:image" content="https://drscdn.500px.org/photo/153331369/q%3D80_m%3D2000/a85e9a59f079b2d7ffe8f96933a11160">
        <meta property="og:description" content="더이상 발품팔지 마세요, 원하시는 방을 대신 찾아드립니다. 원하는 조건을 입력하고 제시된 매물을 온라인에서 살펴>보세요!">

```
