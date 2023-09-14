[참고 문서 링크](https://velog.io/@parkyw1206/React-Typescript%EC%97%90%EC%84%9C-firebase-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0)

1.  Trouble (문제)

    > Typescript 템플릿의 React와 firebase 자동 배포 및 환경 변수 세팅을 순차적으로 아래와 같이 진행해 보았습니다.

2.  Why? (원인)

3.  Solved (해결)

    [실습 결과](https://github.com/JitHoon/react-ts-firebase-test)

    > 1. Firebase 구성원 추가

    구성원 추가를 위해 [Firebase](https://firebase.google.com/?hl=ko) 계정 생성 후, 계정 이메일을 남겨주세요!

    - 재혁님 : ~@gmail.com
    - 특희님 : ~@gmail.com
    - 수연님 : ~@gmail.com
    - 진욱님 : ~@gmail.com

    초대가 완료되면 아래 Firebase App에 접속하실 수 있습니다.

    firebase App 이름 : [techschool-wiki](https://console.firebase.google.com/u/0/project/techschool-wiki/overview) (깃허브 repo이름과 동일)

    2~4 목차는 세팅 진행 과정과 결과를 정리한 내용이 담겨있습니다.

    > 2. React + TS App 생성하기

    ```bash
    	npx create-react-app [프로젝트 이름] —template typescript
    ```

    > 3. Firebase 설치하기

    ```bash
    	npm install firebase
    	npm install -g firebase-tools
    	firebase login
    	firebase init
    ```

    [init 세팅 참고](https://www.notion.so/React-TS-Firebase-8947dc8a67644b0f9569c9e205ff87da?pvs=4#5cac90b933a049c1a8a90d630eaf609e)

    > 4. Firebase App 생성하기

    ```Plain Text
    	.env

    	src
    		ㄴ firebase-config.tsx
    ```

    firebase-config.tsx

    ```typescript
    import { initializeApp } from "firebase/app";
    import { getFirestore } from "firebase/firestore";
    import { getStorage } from "firebase/storage";

    const firebaseConfig = {
      apiKey: "~",
    };

    const app = initializeApp(firebaseConfig);

    export const db = getFirestore(app);
    export const storage = getStorage(app);
    ```

    - 이 파일에서는 `db` 와 `storage` 를 export 합니다.

    1. `db` : 이름, 나이, 사진 URL과 같은 텍스트 데이터 저장소입니다.
    2. `storage` : 이미지, 동영상 저장소입니다.

    - .js 파일과 .tsx 파일의 코드가 동일합니다.
    - src 폴더 최상단에 위치합니다.

    > 5. 데이터와 이미지 Firebase에 업로드하기

    해당 목차는 팀원 분들의 firebase 활용의 이해를 돕기 위한 예시 목차입니다.

    <details>
    <summary>활용 1. db에 데이터 추가하기</summary>
    	
      1. `firebase-config.tsx` 파일에서 `db`를 import한다.
          
          ```tsx
          import { db } from "./firebase-config";
          ```
          
      2. `firebase/firestore` 에서 `collection`, `addDoc` 메서드를 import한다.
          
          ```tsx
          import { collection, addDoc } from "firebase/firestore";
          ```
          
      3. button에 onClick 이벤트 함수를 추가한다.
          
          ```tsx
          <button onClick={createTestDoc}>create test doc</button>
          ```
          
      4. `collection`, `addDoc` 메서드를 활용하여 db에 데이터를 등록한다.
          
          ```tsx
          async function createTestDoc() {
            try {
              await addDoc(collection(db, "tests"), {
                hello: "hello~",
              });
            } catch (err) {
              console.log(err);
            }
          }
          ```
          
      <details>
    	<summary>전체 코드</summary>
          
      ```tsx
      // App.tsx
      // Firestore DB 문서 업로드 예시
      
      import "./App.css";
      import { db } from "./firebase-config";
      import { collection, addDoc } from "firebase/firestore";
      
      function App() {
        return (
          <div className="App">
            <header className="App-header">
              <button onClick={createTestDoc}>create test doc</button>
            </header>
          </div>
        );
      }
      
      async function createTestDoc() {
        await addDoc(collection(db, "tests"), {
          hello: "hello~",
        });
      }
      
      export default App;
      ```
      </details>
    </details>
    <details>
    <summary>활용 2. storage에 이미지 추가하기</summary>

    1.  `firebase-config.tsx` 파일에서 `storage`를 import한다.

        ```tsx
        import { storage } from "./firebase-config";
        ```

    2.  `firebase/firestore` 에서

        `ref`, `uploadBytesResumable`, `getDownloadURL` 메서드를 import한다.

        ```tsx
        import {
          ref,
          uploadBytesResumable,
          getDownloadURL,
        } from "firebase/firestore";
        ```

    3.  input에 onClick 이벤트 함수를 추가한다.

        ```tsx
        <input type="file" onChange={(e) => createTestImg(e)} />
        ```

    4.  `ref`, `uploadBytesResumable`, `getDownloadURL` 메서드를 활용하여
        db에 데이터를 등록한다.

        ````tsx
        function createTestImg(e: any) {
        // input에 얼로드한 파일, 파일명 가져오기
        const file = e.currentTarget.files?.[0];
        const fileName = file.name;

            	// ref 메서드를 활용하여 이미지 저장 파일 경로 설정
              const testImgsRef = ref(storage, `testImgs/${fileName}`);

            	// uploadBytesResumable 메서드를 활용하여 파일 업로드와 업로드 상태 불러오기
              const uploadTask = uploadBytesResumable(testImgsRef, file);

            	// on 메서드를 활용하여 업로드 상태에 따른 동작 처리하기
              uploadTask.on(
                "state_changed",
            		// 업로드 중일 때 실행
                (snapshot) => {
                  console.log("1. snapshot:" + snapshot);
                },
            		// 업로드 실패시 실행
                (err) => {
                  console.log("2. upload test Img err:" + err);
                },
            		// 업로드 완료 후 실행
                () => {
            			// getDownloadURL 메서드를 활용하여 업로드된 사진 url 가져오기
                  getDownloadURL(uploadTask.snapshot.ref)
            				.then((downloadURL) => {
                    console.log("3. downloadURL:" + downloadURL);
                  });
            		}
            	);
            }
            ```
        ````

      <details>
    	<summary>전체 코드</summary>
        
      ```tsx
      // App.tsx
      // Storage 이미지 업로드 예시
      
      import "./App.css";
      import { storage } from "./firebase-config";
      import { ref, uploadBytesResumable, getDownloadURL } from "firebase/storage";
      
      function App() {
        return (
          <div className="App">
            <header className="App-header">
              <input type="file" onChange={(e) => createTestImg(e)} />
            </header>
          </div>
        );
      }
      
      function createTestImg(e: any) {
        try {
          const file = e.currentTarget.files?.[0];
          const fileName = file.name;
          const testImgsRef = ref(storage, `testImgs/${fileName}`);
          const uploadTask = uploadBytesResumable(testImgsRef, file);
      
          uploadTask.on(
            "state_changed",
            (snapshot) => {
              console.log("1. snapshot:" + snapshot);
            },
            (err) => {
              console.log("2. upload test Img err:" + err);
            },
            () => {
              getDownloadURL(uploadTask.snapshot.ref).then((downloadURL) => {
                console.log("3. downloadURL:" + downloadURL);
              });
            }
          );
        } catch (err) {
          console.log("0. createTestImg err: " + err);
        }
      }
      
      export default App;
      ```
      </details>
    </details>

    > 6. firebase + env

    <details>
    <summary>1. 각자의 로컬 환경에서 env가 동작할 수 있도록 설정하기</summary>

    - 모든 OS에서 .env 파일이 동작할 수 있도록 cross-env를 설치
      ```bash
      npm i cross-env
      ```
    - .env 파일 생성
      ```tsx
      // .env

      REACT_APP_API_KEY = DKFSKDF22323
      ...
      ```
    - `process.env.REACT_APP_API_KEY` 으로 사용하기
      ```tsx
      import { initializeApp } from "firebase/app";
      import { getFirestore } from "firebase/firestore";
      import { getStorage } from "firebase/storage";

      const firebaseConfig = {
        apiKey: process.env.REACT_APP_API_KEY,
        authDomain: process.env.REACT_APP_AUTH_DOMAIN,
        projectId: process.env.REACT_APP_PROJECTED_ID,
        storageBucket: process.env.REACT_APP_STORAGE_BUCKET,
        messagingSenderId: process.env.REACT_APP_MESSAGING_SENDER_ID,
        appId: process.env.REACT_APP_APP_ID,
        measurementId: process.env.REACT_APP_MEASUREMENT_ID,
      };

      const app = initializeApp(firebaseConfig);

      export const db = getFirestore(app);
      export const storage = getStorage(app);
      ```

    [참고 자료](https://han-py.tistory.com/441)
    </details>

    <details>
    <summary>2. firebase 배포 환경에서 환경 변수 설정하기</summary>

    - github repo → setting → Enviroment secrets 에 환경 변수 추가하기

          ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3ef8dbd9-414c-4cf5-813d-32ecb943cc67/658cc4ca-60d0-4d18-95f2-abc621ff7303/Untitled.png)

    - workflow에 secrets 환경 변수 불러와 사용하기

      ```bash
      # This file was auto-generated by the Firebase CLI
      # https://github.com/firebase/firebase-tools

      # firebase-hosting-merge.yml

      name: Deploy to Firebase Hosting on merge
      "on":
        push:
          branches:
            - main
      jobs:
        build_and_deploy:
          runs-on: ubuntu-latest
          environment: firebase-deploy
          env:
            REACT_APP_API_KEY: "${{ secrets.REACT_APP_API_KEY }}"
            REACT_APP_AUTH_DOMAIN: "${{ secrets.REACT_APP_AUTH_DOMAIN }}"
            REACT_APP_PROJECTED_ID: "${{ secrets.REACT_APP_PROJECTED_ID }}"
            REACT_APP_STORAGE_BUCKET: "${{ secrets.REACT_APP_STORAGE_BUCKET }}"
            REACT_APP_MESSAGING_SENDER_ID: "${{ secrets.REACT_APP_MESSAGING_SENDER_ID }}"
            REACT_APP_APP_ID: "${{ secrets.REACT_APP_APP_ID }}"
            REACT_APP_MEASUREMENT_ID: "${{ secrets.REACT_APP_MEASUREMENT_ID }}"
          steps:
            - uses: actions/checkout@v3
            - run: npm ci && npm run build
            - uses: FirebaseExtended/action-hosting-deploy@v0
              with:
                repoToken: "${{ secrets.GITHUB_TOKEN }}"
                firebaseServiceAccount: "${{ secrets.FIREBASE_SERVICE_ACCOUNT_TECHSCHOOL_WIKI }}"
                channelId: live
                projectId: techschool-wiki
      ```

      ```bash
      # This file was auto-generated by the Firebase CLI
      # https://github.com/firebase/firebase-tools

      # firebase-hosting-pull-request.yml

      name: Deploy to Firebase Hosting on PR
      "on": pull_request
      jobs:
        build_and_preview:
          if: "${{ github.event.pull_request.head.repo.full_name == github.repository }}"
          runs-on: ubuntu-latest
          env:
            REACT_APP_API_KEY: "${{ secrets.REACT_APP_API_KEY }}"
            REACT_APP_AUTH_DOMAIN: "${{ secrets.REACT_APP_AUTH_DOMAIN }}"
            REACT_APP_PROJECTED_ID: "${{ secrets.REACT_APP_PROJECTED_ID }}"
            REACT_APP_STORAGE_BUCKET: "${{ secrets.REACT_APP_STORAGE_BUCKET }}"
            REACT_APP_MESSAGING_SENDER_ID: "${{ secrets.REACT_APP_MESSAGING_SENDER_ID }}"
            REACT_APP_APP_ID: "${{ secrets.REACT_APP_APP_ID }}"
            REACT_APP_MEASUREMENT_ID: "${{ secrets.REACT_APP_MEASUREMENT_ID }}"
          steps:
            - uses: actions/checkout@v3
            - run: npm ci && npm run build
            - uses: FirebaseExtended/action-hosting-deploy@v0
              with:
                repoToken: "${{ secrets.GITHUB_TOKEN }}"
                firebaseServiceAccount: "${{ secrets.FIREBASE_SERVICE_ACCOUNT_TECHSCHOOL_WIKI }}"
                projectId: techschool-wiki
      ```

    [참고 자료](https://mye280c37.tistory.com/19)

    - test 코드 (App.tsx)

      ````tsx
      import React, {useEffect} from "react";
      // import {Routes, Route} from "react-router-dom";
      import "./styles/App.css";
      import "./styles/reset.css";
      import {collection, addDoc, getDocs} from "firebase/firestore";
      import {ref, uploadBytesResumable, getDownloadURL} from "firebase/storage";
      import {db, storage} from "./utils/firebaseConfig";

          function App() {
            async function createTestDoc() {
              await addDoc(collection(db, "tests"), {
                hello: "hello~",
              });
            }

            function createTestImg(e: any) {
              try {
                const file = e.currentTarget.files?.[0];
                const fileName = file.name;
                const testImgsRef = ref(storage, `testImgs/${fileName}`);
                const uploadTask = uploadBytesResumable(testImgsRef, file);

                uploadTask.on(
                  "state_changed",
                  (snapshot: any) => {
                    throw snapshot;
                  },
                  (err: any) => {
                    throw err;
                  },
                  () => {
                    getDownloadURL(uploadTask.snapshot.ref).then((downloadURL: any) => {
                      throw downloadURL;
                    });
                  },
                );
              } catch (err: any) {
                throw new Error(err);
              }
            }
            useEffect(() => {
              async function loadTest() {
                const imagesCollection = collection(db, "tests");
                try {
                  const querySnapshot = await getDocs(imagesCollection);
                  querySnapshot.forEach(docs => {
                    const data = docs.data();
                    const item = data.text;
                    localStorage.setItem("data", item);
                  });
                } catch (err: any) {
                  throw new Error(err);
                }
              }

              loadTest();
            }, []);
            return (
              <div className="App">
                <header className="App-header">
                  <button type="button" onClick={createTestDoc}>
                    create test doc
                  </button>
                  <input type="file" onChange={e => createTestImg(e)} />
                </header>
              </div>
            );
          }

          export default App;
          ```

      </details>
      ````
