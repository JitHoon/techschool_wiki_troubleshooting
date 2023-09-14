[참고 문서 링크](https://stackoverflow.com/questions/52130772/whats-the-purpose-of-firebase-hosting-alphanum-cache)

1. Trouble (문제)

   > .firebase/hosting.ALPHANUM.cache 파일을 main에 push할 필요가 있는지 확인하기 위해 .firebase 파일의 역할을 찾아보았습니다.

2. Why? (원인)

   > hosting.ALPHANUM.cache 파일의 역할은 firebase의 자동 배포를 빠르게 할 수 있도록 하는 것입니다.

   - firebase는 해당 파일을 통해 이전 배포와 달라진 점을 비교하여 변경된 부분만 수정하는 방식으로 빠르게 배포를 할 수 있습니다.

3. Solved (해결)

   > hosting.ALPHANUM.cache 파일 뿐만 아니라 .firebase 파일을 .giignore 파일에 추가

   - hosting.ALPHANUM.cache 파일은 배포마다 변하는 파일이고 firbase 자동 배포에만 사용되므로 모든 팀원이 공유할 필요가 없습니다.
   - .firebase 파일에 생성되는 파일들이 없다고해서 프로젝트에 큰 영향을 주지 않으며, 모든 팀원이 공유할 필요가 없습니다.
