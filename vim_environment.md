# VIM을 이용한 개발환경

## VIM 기본 사용방법

### vim + ctags + cscope 설치하기

> $ sudo apt-get install vim ctags cscope

- vim + ctags + cscope의 기본사용법은 다음의 링크를 참고하세요
[http://csl.skku.edu/uploads/SSE3044F12/vim_ctags_cscope.pdf](http://csl.skku.edu/uploads/SSE3044F12/vim_ctags_cscope.pdf/)
[https://opentutorials.org/module/522](https://opentutorials.org/module/522/)

## Vim 환경설정

### vim 에디터 환경설정

vim에서는 에디터의 환경 (ex. 탭간격설정, syntax highlight 등)을 .vimrc 파일에 설정할 수 있습니다.
lab 서버에 저장되어 있는 템플릿 파일을 이용해도 됩니다.

### ctags 태그 생성

해당프로그램의 최상위 폴더로 이동후 다음의 명령어를 실행합니다.

> ctags -R

-R 옵션은 하위디렉토리까지 포함하여 태그를 생성합니다.

위의 명령어를 이용해 만들어진 tag를 vim에 로드해주기 위해서 [명령 모드]나 .vimrc 파일에 아래의 예제와 같이 입력/추가해주어야 vim에서 생성된 tag를 이용할 수 있습니다.

```{.no-highlight}
set tags+=~/irap/tags
```

### cscope 데이터베이스 생성방법

cscope 데이터베이스를 생성하기위해서는 두 단계가 필요합니다. find 명령을 이용해서 파일 목록을 생성/저장하고, 저장된 파일목록을 불러와서 cscope -i [파일명] 명령어를 이용하여 데이터베이스 파일을 생성합니다.

아래의 내용을 script파일로 만들어 간단히 수행해줄수도 있습니다.

```{.no-highlight}
#!/bin/sh
rm -rf cscope.files cscope.files
find `pwd` \( -name '*.c' -o -name '*.cpp' -o -name '*.cc' -o -name '*.h' -o -name '*.hpp' -o -name '*.s' -o -name '*.S' \) -print>cscope.files
cscope -i cscope.files
```

## VIM plugin 사용

### VIM plugin 다운로드

vim 플러그인은 아래의 링크에서 다운받을 수 있습니다. 현재 사용해본 바로는 Taglist / NERDTree / SrcExpl등이 유용한듯합니다. 서버에 있는 환경설정파일을 이용하면 [F6], [F7], [F8]이 키맵핑 되어있어서 vim을 실행하면 해당 키를 이용하여 plugin을 바로 호출할 수도 있습니다.

[http://www.vim.org/scripts/script_search_results.php](http://www.vim.org/scripts/script_search_results.php/)


