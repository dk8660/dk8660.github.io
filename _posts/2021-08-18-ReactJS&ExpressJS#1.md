---
title: "[모각코]프로젝트 외출 진행 사항: ReactJS with ExpressJS #1"
tags:
  - ReactJS
  - ExpressJS
  - Project_oc
excerpt: ""
---

# ReactJS with ExpressJS #1

현재 백 엔드 개발이 모두 완료된 상태이기 때문에 이제는 프론트 엔드와 연결해줄 차례가 왔다.

아직 프론트 엔드 개발이 끝나지는 않아 당장 실행할 수는 없어서 먼저 어떻게 연결해줄 지를 찾아보았다.



일단 서버는 백 엔드로 만든 사용자가 요청하는 html 페이지를 보내주는 프로그램이라고 볼 수 있다. 우리는 Express로 만든 서버를 통해 사용자에게 React로 만든 html을 보내주는 것으로 Express와 React를 연동시킬 수 있다.



리액트로 개발을 모두 마친 후에 `npm run build`를 통해 html, css, js 파일을 얻을 수 있다. 이렇게 얻은 파일들을 서버에 적용함으로써 하나로 프론트와 백을 연결시킬 수 있다.



하지만 지금 내가 작성한 프로그램은 많은 양의 html을 사용중이고, React에서 얻는 html은 index.html 단 한 개이기 때문에 어떻게 실제로 연동시킬 지는 직접 해봐야 알 수 있을 것 같다.
