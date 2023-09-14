[참고 문서 링크1](https://stackoverflow.com/questions/75514653/firebase-action-hosting-deploy-fails-with-requesterror-resource-not-accessible)

[참고 문서 링크2](https://github.com/github/codeql-action/issues/572)

1. Trouble (문제)

   > main branch에 merge 후 실행되는 firebae의 자동 배포가 RequestError [HttpError]: Resource not accessible by integration 에러로 중단됨

   ![Untitled (1)](https://github.com/JitHoon/techschool_wiki_troubleshooting/assets/101972330/1c9f00de-c852-4d9f-8b05-7a59ef40d467)

2. Why? (원인)

   > firestore 호스팅 과정에서 사용되는 Workflows가 repository의 scopes에 대한 Read Write 권한이 없어서 배포 중단.

3. Solved (해결)

   > 깃허브 Workflow Permission section 설정에서 Workflows의 repository의 scopes에 대한 Read Write 권한부여

   ![Untitled](https://github.com/JitHoon/techschool_wiki_troubleshooting/assets/101972330/c18e44f5-e32c-4c4a-8f63-21f3a34cc7cb)
