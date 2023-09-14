[참고 문서 링크](https://velog.io/@jurije/FIRESTORE-INTERNAL-ASSERTION-FAILED-Invalid-document-reference.-Document-references-must-have-an-even-number-of-segments)

1. Trouble (문제)

   > firebase document를 get하는 과정에서 데이터는 잘 불러와 지는데, 아래 에러가 console에 지속적으로 던져짐

   - Document references must have an even number of segments

   ```tsx
   ReadContent(text).then((textContent: string) => {
     setContent(textContent);
   });
   ```

2. Why? (원인)

   > text state가 uri 파라미터를 가져오기도 전에 firebase document를 get하려고 시도함.

3. Solved (해결)

   > text state가 uri 파라미터를 가져오면(빈값이 아니면) firebase document를 get하도록 변경

   ```tsx
   if (text) {
     ReadContent(text).then((textContent: string) => {
       setContent(textContent);
     });
   }
   ```
