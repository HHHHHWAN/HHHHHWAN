### `multer` 파일 originalname 한글 인코딩 문제


- **헤더 정보를 `latin1`로 인코딩하여, 출력이 하기 때문**

    [ 설명 (chatGPT)](https://github.com/HHHHHWAN/HHHHHWAN/blob/master/Node.js/multer/%EC%99%9C%20%EB%9D%BC%ED%8B%B4%EB%B0%A9%EC%8B%9D%EC%9D%B4%20%EA%B8%B0%EB%B3%B8%EC%9D%B8%20%EA%B2%83%EC%9D%BC%EA%B9%8C.md)


`multer`를 통해, 저장된 req.file.originalname을 다시 <br>
`latin1` → `hex` → `ASCII/utf-8` 로 인코딩 한 후, 따로 저장할 필요가 있다.



```
//multer storage 기능 
const setting = multer.diskStorage({
    destination : ( req, file, callback ) => {
        callback(null, upload_path);
    },
    filename : ( req, file, callback ) => {
        const decodedName = Buffer.from(file.originalname, 'latin1').toString('utf8');
        // file.originalname을 다시 hex 값으로 변환 후, `toString`으로 utf8로 변환 filename에 저장한다. 
        callback(null, decodedName);
    }
});
```
*filename은 따로 지정하지 않을 경우, 랜덤 숫자로 저장된다.*



![Image](https://github.com/user-attachments/assets/b8bee088-f001-4120-9cec-7ae2236a9716)

정상 출력되는 `filename`