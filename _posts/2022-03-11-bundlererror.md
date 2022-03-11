---
title : "There are no gemspecs at 오류 발생시 해결방안"
categories:
 - ETC
tags:
 - [ etc ] 

toc: true
toc_sticky: true

date: 2022-03-11
last_modified_at: 2022-03-11

---

깃허브 블로그를 local에서 실행하려고 해당 명령어를 입력한 순간,

```bash
bundle exec jekyll serve 
```
<br><br>

아래와 같은 에러가 발생했다.

<figure style="display:block; text-align:center;">
    <img src="/assets/images/bundler_error.png" width="90%" height="90%" title="bundler실행시 에러" alt="bundler실행시 에러나는 화면"/> 
        <figcaption style="text-align:center; font-size:15px; color:#f00">
            [!] There was an error parsing `Gemfile`: There are no gemspecs at /home/ashley/seolhee-choi.github.io. Bundler cannot continue.
        </figcaption><br>
    <img src="/assets/images/gemfile.png" width="90%" height="10%" title="gemfile선택" alt="Gemfile선택"/> 
        <figcaption style="text-align:center; font-size:15px; color:#000">
            사진에 있는 Gemfile을 눌러서 들어간뒤
        </figcaption><br>
    <img src="/assets/images/gemfile2.png" width="90%" height="90%" title="gemfile내용 수정1" alt="gemfile내용 수정1"/> 
        <figcaption style="text-align:center; font-size:15px; color:#000">
            #gemspec 하단에 아래 3줄을 추가했다.
        </figcaption><br>
    <img src="/assets/images/gemfile3.png" width="90%" height="90%" title="gemfile내용 수정2" alt="gemfile내용 수정2"/> <br><br>
    <img src="/assets/images/bundler_success.png" width="90%" height="90%" title="bundler성공시 화면" alt="bundler성공시 화면"/> 
        <figcaption style="text-align:center; font-size:15px; color:#000">
            다시 'bundle exec jekyll serve' 명령어 입력해보면 정상으로 작동되는 것이 확인됨     
        </figcaption>
</figure>

