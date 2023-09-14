[참고 문서 링크](https://notlonely.tistory.com/182)

1. Trouble (문제)

   > file type의 input 사진 값을 React에서는 어떻게 가져올 수 있는지 공부해 보았습니다.

2. Why? (원인)

3. Solved (해결)

   > onChange에서 넘겨주는 event 값에서 file을 받아옵니다.

   ```tsx
   function App() {
     return (
       <div className="App">
         <header className="App-header">
           <input type="file" onChange={(e) => createDriverImg(e)} />
         </header>
       </div>
     );
   }

   export default App;

   function createDriverImg(e: any) {
     const file = e.currentTarget.files?.[0];
     const fileName = file.name;
     const testImgsRef = ref(storage, `testImgs/${fileName}`);
   }
   ```
