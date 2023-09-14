[참고 문서 링크](https://space-rumi.tistory.com/102#google_vignette)

1. Trouble (문제)

   > props를 넘겨줄 때 IntrinsicAttributes type error 발생

   ```tsx
   // Content.tsx
   import MainContent from "./MainContent";

   <MainContent text={text} editTrue={editTrue} />;
   // Type '{ text: string, editTrue: boolean }' is not assignable to type 'IntrinsicAttributes'.
   ```

   ```tsx
   // MainContent.tsx
   import React, {useState} from "react";

   function MainContent({text, editTrue}) {
   	if(editTrue){
   		...
   	}

     return (
       <div id="main-content">
         {text}
       </div>
     );
   }

   export default MainContent;
   ```

2. Why? (원인)

   > MainContent의 인자인 {text, editTrue} props의 타입을 명시하지 않음

3. Solved (해결)

   > {text, editTrue} props의 타입을 interface로 타입 명시

   ```tsx
   // types/Wiki.tsx

   export interface MainContentProps {
     text: string;
     editTrue: boolean;
   }
   ```

   ```tsx
   // MainContent.tsx

   import React, {useState} from "react";
   import {MainContentProps} from "../../types/Wiki";

   function MainContent({text, editTrue}: MainContentProps) {
   	if(editTrue){
   		...
   	}

     return (
       <div id="main-content">
         {text}
       </div>
     );
   }

   export default MainContent;
   ```
