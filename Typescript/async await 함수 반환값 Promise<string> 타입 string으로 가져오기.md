[참고 문서 링크](https://bobbyhadz.com/blog/typescript-type-promise-is-not-assignable-to-type)

1. Trouble (문제)

   > <ReactMarkdown> 컴포넌트 사이에 string으로 데이터를 넘겨줘야하는 상황에서, Firebase의 Document에 있는 string 타입의 field를 return하는 async await 함수를 <ReactMarkdown> 컴포넌트 사이에 바로 넣어 사용할 수 없었습니다.

   ```tsx
   // ReadContent.tsx

   import { doc, getDoc } from "firebase/firestore";
   import { db } from "../../utils/firebaseConfig";

   async function ReadContent(text: string): Promise<string> {
     const wikiRef = doc(db, "wiki", text);
     const wikiSnap = await getDoc(wikiRef);
     const wiki = wikiSnap.data();

     return wiki?.content;
   }

   export default ReadContent;
   ```

   ```tsx
   // Content.tsx

   <ReactMarkdown>{ReadContent(text)}</ReactMarkdown>
   ```

2. Why? (원인)

   > async await 함수는 상항 Promise<string> 타입을 반환하기 때문에 Firebase field 데이터로 string으로 값을 넘겨 받는다 하더라도 Promise<string> 타입으로 반환합니다.

3. Solved (해결)

   > content state를 생성하여 ReadContent(text) 반환값을 then()으로 받아와 string 값으로 content에 넣어주었습니다. 이렇게 받아온 content를 <ReactMarkdown> 컴포넌트에 넘겨주었습니다.

   ```tsx
   // Content.tsx

   const [content, setContent] = useState("");

   ReadContent(text).then((textContent: string) => {
     setContent(textContent);
   });

   <ReactMarkdown>{ReadContent(text)}</ReactMarkdown>;
   ```
