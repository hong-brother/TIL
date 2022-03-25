>
리눅스의 USB 메모리에서 하드로 파일을 이동할때 나느 cross-device link not permitted 에 대해서 정리 해본다.

## 원인
### cross-device link not permitted 란
- 리눅스에서 rename으로 파일을 다른 디바이스로 이동할때 에러가 발생하는 문제 입니다. rename 명령어는 기본적으로 동일한 디바이스에서 처리되는 명령어 입니다.


## 해결
- 파일을 stream으로 읽어서 stream으로 write해주고 완료되면 이전파일은 삭제 시켜주면 된다.
```javascript
	const rs = fs.createReadStream(tempImageFile);
	const ws = fs.createWriteStream(uploadImageFile);
	rs.pipe(ws);
    rs.on('end', () => {
      fs.unlinkSync(tempImageFile);
    });
```
- 또한 copyFile 함수로 문제를 해결 할 수 있다.
```javascript
  fs.copyFile(tempImageFile, uploadImageFile, (err) => {
    fs.unlink(tempImageFile, (err) => {});
    if (err) console.log(err)
  });
```
