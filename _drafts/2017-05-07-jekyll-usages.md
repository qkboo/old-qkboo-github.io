---
title: Jekyll Usages
date: 2017-05-07 10:10:00 +0900
layout: post
tags: jekyll
categories: jekyll
---

# jekyll 기본 사용

- 지킬 사이트의 테마 이용
- 전역 설정
- 글쓰기
- 사이트 변환


## New site

new 명령을 jekyll의 gem-based themes 를 사용하게 구성해 준다. 혹은 빈 폴더에서 새롭게 구성할 수 있다.

**jekyll new**로 생성되는 사이트는 gem-based theme를 사용한 jekyll project bootstrapped 로 생성된다.

jekyll 디렉토리 구조


```.
├── _config.yml
├── _data
|   └── members.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.md
|   └── on-simplicity-in-technology.md
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
|   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
|   ├── _base.scss
|   └── _layout.scss
├── _site
├── .jekyll-metadata
└── index.html # can also be an 'index.md' with valid YAML Frontmatter```

| 파일 | 설명 |
|----------|-----------------------------------|
|_config.yml | 설정 파일 |
|_drafts | 발행하지 않은 준비중인 포스트. |
| _includes | 레이아웃과 포스트에 포함시킬... 재사용 가능한 조각들 |
| _layouts |  |


### new 명령

*jekyll new SITE_NAME* 으로 생성시 아래 같은 템플릿으로 구성.

```sh
Gemfile
Gemfile.lock
_config.yml
_posts/
_site/
about.md
index.md
```

### 빈 사이트 만들기


> jekyll new myblog --blank


```
_draft/
_layout/
_posts/
index.html
```


##### **_config.yml**

[jekyll configuration](https://jekyllrb.com/docs/configuration/) 참조
[jekyll 설정](http://jekyllrb-ko.github.io/docs/configuration/) 참조


```yml
markdown: kramdown
highlighter: pygments
permalink: pretty
relative_permalinks: false
```


rouge syntax highlighter
https://sacha.me/articles/jekyll-rouge/

Jekyll 실행 환경을 지정할 수 있다. 
예를 들어 디버깅, 개발, 상품 환경으로 구분한다면 코드에 다음 같이 넣을 수 있다:

{% raw %}
```liquid
{% if jekyll.environment == "production" %}
  {% include disqus_comments.html %}
{% endif %}
```
{% endraw %}

https://shopify.github.io/liquid/basics/introduction/
http://ozzieliu.com/2016/04/26/writing-liquid-template-in-markdown-with-jekyll/


jekyll을 실행시 JEKYLL_ENV에 값을 지정해 줄 수 있다. 기본 값은 development 이다.

```sh
$ JEKYLL_ENV=production bundle exec jekyll build
$ JEKYLL_ENV=production jekyll build
```

지킬의 포스트 등에서 사용하는 변수는 https://jekyllrb.com/docs/variables/ 에서 확인할 수 있다.


##### github 지원 config items

아래는 GitHub에서 기본으로 제공하는 설정으로 사용자가 원하는 대로 변경이 가능한 설정입니다.

```yaml
 github: [metadata]
 kramdown:
   input: GFM
   hard_wrap: false
 gems:
   - jekyll-coffeescript
   - jekyll-paginate

 lsi: false
 safe: true
 source: [your repo's top level directory]
 incremental: false
 highlighter: rouge
 gist:
   noscript: false
```


- lsi : 관련 포스트글에 대한 인덱스를 생성합니다.
- safe : 사용자 플러그인을 비활성화하고, 심볼릭 링크(symbolic links)를 무시합니다.
- source : Jekyll이 읽을 파일의 위치를 변경합니다.
- incremental : 변경된 포스트만 재빌드하는 옵션입니다.
- highlighter : GitHub에서 자체적으로 설정하는 코드 하이라이터인 것 같습니다. Jekyll 문서에는 따로 설명이 없습니다.
- gist : 이것도 GitHub에서 자체 설정인 것 같습니다.



#### **post**

포스트는 **_posts** 폴더에 저장한다. **yyyy-mm-dd-파일명.markup**( md 또는 markdown 또는 textile) 형식으로 포스트 파일명을 만들어야 한다.

파일 내용은 다음 같이 구성된다.

> Front matter
> BODY

Front matter 는 다음 같이 구성되고, **[Front Matter](http://jekyllrb.com/docs/frontmatter/)**, **[머리말](http://jekyllrb-ko.github.io/docs/frontmatter/)**를 참조한다.

YAML 머리말 블록을 가진 모든 파일을 특별한 파일로 인식하여 처리합니다. 머리말은 반드시 올바른 YAML 형식으로 작성되어야 하며, 대시문자 3 개로 감싸서 파일의 맨 첫 부분에 위치해야 합니다.

BODY 내용은 마크다운, 기타 문법 형식으로 작성하면 된다.

> []다른 포맷 지원](http://jekyllrb-ko.github.io/docs/plugins/#converters-1)

##### Front Matter 머릿말

세 개의 대쉬 라인(---) 사이에 메타 정보를 넣는다.

```yaml
---
layout: post
title:  "Welcome to Jekyll!"
date:   2017-05-03 18:53:47 +0900
categories: jekyll update
---
```
- 빈 메타 정보는 빈 두개의 --- 로 둔다.

Front matter 에 사용할 수 있는 내장된 변수는 다음 같다:

|변수|설명|
|------------|-------------------------------------------|
|**layout**|사용할 레이아웃 파일을 지정한다. 레이아웃 파일명에서 확장자를 제외한 나머지 부분만 입력한다. 레이아웃 파일은 반드시 _layouts 디렉토리에 존재해야 한다.|
|**permalink**|생성된 블로그 포스트 URL 을 사이트 전역 스타일 (디폴트 설정: /year/month/day/title.html)이 아닌 다른 스타일로 만드려면, 이 변수를 사용하여 최종 URL 을 설정하면 된다.|
|**published**|사이트가 생성되었을 때 특정 포스트가 나타나지 않게 하려면 false 로 설정하라.|
|**category,categories**|포스트를 특정 폴더에 넣지 않고, 포스트가 속해야 하는 카테고리를 하나 또는 그 이상 지정할 수 있다. 사이트가 생성될 때, 포스트는 그냥 평범하게 이 카테고리들에 속한 것처럼 동작한다. 두 개 이상의 카테고리들을 지정할 때에는 YAML 리스트 또는 쉼표로 구분된 문자열을 사용한다.|
|**tags**|카테고리와 유사하게, 하나 이상의 태그를 포스트에 추가할 수 있다. 또 카테고리와 동일하게, YAML 리스트 또는 쉼표로 구분된 문자열로 지정할 수도 있다.|



#### Syntaxhighlighting

https://gist.github.com/zakkain/3203448

https://demisx.github.io/jekyll/2014/01/13/improve-code-highlighting-in-jekyll.html



##### 외부 자원

이미지, 다운로드 파일 등을 사용할 때는 루트 디렉토리의 `images`, `assets`, `downloads` 라는 디렉토리를 만들고 그곳에 둔다. 그리고 해당 자원의 참조를 */* 경로를 기준으로 삼으면 된다.

```
![친절한 스크린샷](/screenshot.jpg)
```

##### site.url 변수

```
![친절한 스크린샷]({{ site.url }}/assets/screenshot.jpg)
```


```
… PDF 를 직접 [다운로드]({{ site.url }}/assets/mydoc.pdf)할 수 있습니다.

```

##### Build

실제 웹 사이트에는 html 파일로 제공되야 한다. 그러기 위해서 *serve* 혹은 *build* 명령으로 마크다운 파일을 변환해야 한다.

> jekyll serve 명령은 지킬 사이트 디렉터리 안으로 접근하여 실행해야 한다.

*serve* 명령으로 빌드한 html과 파일은 _site 폴더에 생성된다.



## Theme

Jekyll은 기본 테마로 `Minima`라 불리는 gem-based theme를 사용한다. 이 테마를 구성하는 파일은 jekyll new <PATH> 명령으로 <PATH> 위치에 다음 같이 구성된다.

> Minima 테마는 assets, _layouts, _includes, and _sass 디렉토리를 실제 Minima theme gem 디렉토리에 위치하고 있고 아래 같은 구성으로 사이트가 생성된다.


```
├── Gemfile
├── Gemfile.lock
├── _config.yml
├── _posts
│   └── 2016-12-04-welcome-to-jekyll.markdown
├── about.md
└── index.md
```

다른 Theme gem을 사용하려면 `bundle update`를 실행하거나 `bundle update <THEME>` 로 사용할 <THEME> 를 지정한다.
jekyll은 사이트 접근시 처음에 컨텐츠를 아래 폴더 안에서 찾는다.
- /assets
- /_layouts
- /_includes
- /_sass

예를 들어 `post` 레이아웃을 사용하고 있다면 *_layouts* 폴더에 **_layouts/page.html** 테마 파일을 생성해 변경할 수 있다. 처음부터 생성하는 것 보다 기본 테마 파일을 이용하는 것이 빠르다.

### 기본테마 Manima

Minima 테마의 기본 폴더는 `bundle show minima` 명령으로 확인이 가능하다. 기본테마 디렉토리는 아래 같이 구성되어 있다. 테마 정의에 필요한 *_includes/, _layouts/, _sass/, assets/* 폴더이다.

```yaml
 ├── _includes
 │   ├── disqus_comments.html
 │   ├── footer.html
 │   ├── google-analytics.html
 │   ├── head.html
 │   ├── header.html
 │   ├── icon-github.html
 │   ├── icon-github.svg
 │   ├── icon-twitter.html
 │   └── icon-twitter.svg
 ├── _layouts
 │   ├── default.html
 │   ├── home.html
 │   ├── page.html
 │   └── post.html
 ├── _sass
 │   ├── minima
 │   │   ├── _base.scss
 │   │   ├── _layout.scss
 │   │   └── _syntax-highlighting.scss
 │   └── minima.scss
 └── assets
     └── main.scss
```


#### 기본 테마 재정의 하기

Jekyll theme는 기본 layouts, includes, stylesheets를 지정하는데, 이것을 사이트 콘텐트에 맞게 재정의할 수 있다.

Minima 테마의 기본 폴더는 `bundle show minima` 명령으로 확인이 가능하다. 그리고 아래 같이 찾아서 열어 볼 수 있다. 먼저 macOS 는

```sh
open $(bundle show minima)
```

Windows 에서는

```
explorer /usr/local/lib/ruby/gems/2.3.0/gems/minima-2.1.0
```

기본 테마 디렉토리 구조를 복사해 와서 작업하겠다.

```sh
$ cd `bundle show minima`
~minima-2.1.1$ cp -r _includes _layouts _sass assets ~/mysite/
```

각각의 테마 요소를 알아보자.

#### Layout

컨텐츠의 구성은 **_layouts** 폴더에 넣는다. 이렇게 구성해 보자

```
default.html|
            ├<-- post.html
            ├<-- page.html
```


##### default.html

```
<!DOCTYPE html>
<html>

  {% include head.html %}

  <body>

    {% include header.html %}

    <div class="page-content">
      <div class="wrapper">
        {{ content }}
      </div>
    </div>

    {% include footer.html %}

    {% include google-analytics.html %}

    <script id="dsq-count-scr" src="//{{ site.disqus }}.disqus.com/count.js" async></script>

  </body>

</html>

```


##### page.html

```
---
layout: default
---
<article class="post">

  <header class="post-header">
    <h1 class="post-title">{{ page.title }}</h1>
  </header>

  <div class="post-content">
    {{ content }}
  </div>

</article>

```


##### post.html

```yaml
---
layout: default
---
<article class="post" itemscope itemtype="http://schema.org/BlogPosting">

  <header class="post-header">
    <h1 class="post-title" itemprop="name headline">{{ page.title }}</h1>
    <p class="post-meta"><time datetime="{{ page.date | date_to_xmlschema }}" itemprop="datePublished">{{ page.date | date: "%b %-d, %Y" }}</time>{% if page.author %} • <span itemprop="author" itemscope itemtype="http://schema.org/Person"><span itemprop="name">{{ page.author }}</span></span>{% endif %}{% if page.comments %} • <a href="https://xho95.github.io{{ page.url }}#disqus_thread">0 Comments</a>{% endif %}</p>
  </header>

  <div class="post-content" itemprop="articleBody">
    {{ content }}
  </div>
</article>

```


#### Social icons

github, twitter 등의 아이콘을 포스트 글의 아래에 배치해 보자

##### footer.html

include/footer.html 를 사용한다고 가정하고,,


```
<div class="footer-col footer-col-2">
        <ul class="social-media-list">
          {% if site.github_username %}
          <li>
            {% include icon-github.html username=site.github_username %}
          </li>
          {% endif %}

          {% if site.twitter_username %}
          <li>
            {% include icon-twitter.html username=site.twitter_username %}
          </li>
          {% endif %}
        </ul>
      </div>
```

역시 include에 icon-github.html, icon-twitter.html, icon-facebook.html 페이지를 만든다.

icon-github.html

```yaml
<a href="https://github.com/{{ include.username }}"><span class="icon icon--github">{% include icon-github.svg %}</span><span class="username">{{ include.username }}</span></a>

```



#### sass

*_sass* 디렉토리에 *.sass* 파일을 두면 sass 컴파일러가 컴파일 한다.



### Disqus

disqus.com 에서 새 사이트를 구성하고, Jekyll 을 선택하면 Universal code 를 얻을 수 있다.


#### Disqus Universal Code 설치

1. comments 변수

`comments` 변수를 YAML Front Matter에 추가하기 위해, Jekyll의 manima 테마에서 _layouts/post.html 에 변수를 추가해 준다.

```yaml
---
layout: default
comments: true
---
```


2. Universal code

```{% if page.comments %}``` 와 ```{% endif %}``` 태그 사이에 Universal Embeded Code를 추가해 준다. manima 테마의 _includes/disqus_comments.html 파일에 구성되어 있다.  코드에서production을 developement로 바꿔서 테스트 해보자.

#### 댓글 수 표시

</body> 태그 전에 아래 스크립트를 원하는 위치에 둔다.

```
{% if page.comments %}
  <!-- Disqus comment count -->
  <script id="dsq-count-scr" src="//{{ site.disqus.shortname }}.disqus.com/count.js" async></script>
{% endif %}

```

href 속성에 #disqus_thread 를 추가하기 위해서, _layouts/post.html 템플릿에 다음을 추가한다.

```
{% if page.author %}
• <span itemprop="author" itemscope itemtype="http://schema.org/Person"><span itemprop="name">{{ page.author }}</span></span>
{% endif %}
{% if page.comments %}
  • <a href="{{ site.url }}{{ page.url }}#disqus_thread">Comments</a>
{% endif %}
```

![](/images/jekyll/disqus-comment-count.png)

조건에 page.conmments 를 참조하면 post 에 `comments: true` 가 정의되면 되고, layout 전체를 담당하려면 아래 같은 layout.comments 를 비교한다:

```yaml
{% if layout.comments %}
  • <a href="{{ site.url }}{{ page.url }}#disqus_thread">Comments</a>
{% endif %}
```


### Google Analytics

default.html

```
<!DOCTYPE html>
<html>

  {% include head.html %}

  <body>

    {% include header.html %}

    <div class="page-content">
      <div class="wrapper">
        {{ content }}
      </div>
    </div>

    {% include footer.html %}

    {% include google-analytics.html %}

    <script id="dsq-count-scr" src="//{{ site.disqus }}.disqus.com/count.js" async></script>

  </body>

</html>

```



## github pages