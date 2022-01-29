### 해당 Directory 내에 모든 파일 찾기
- 해당 Directory안의 하위 폴더 안까지 검색하여 특정파일 찾는 방법이다.
먼저 파일을 찾으면 정보를 넣어줄 클래스 정의한다.
- FileInfoDto Dto 클래스 정보
```javascript
export class FileInfoDto {
  originalname: string;
  encoding: string;
  mimetype: string;
  size: number;
  destination: string;
  filename: string;
  path: string;
  buffer: Buffer;
}
```
- 검색할 파일 경로와 파일정보를 담아줄 list를 파라미터로 정의하고 fs.readdirSync를 통하여 해당 directory를 읽는다 이때. withFileTypes: true 옵션을 통해 반환되는 객체에 isDirectory()를 사용 할 수 있다. 
>
[document](https://nodejs.org/api/fs.html#fsreaddirsyncpath-options)
If options.withFileTypes is set to true, the resolved array will contain <fs.Dirent> objects.
```javascript
async inspectionFindFile(destPath, list: FileInfoDto[]) {
    try {
      await fs
        .readdirSync(destPath, { withFileTypes: true })
        .forEach((file) => {
          const filePath = `${destPath}/${file.name}`;

          if (file.isDirectory()) {
            this.inspectionFindFile(filePath, list);
          } else {
            const ext = path.extname(filePath).toLowerCase();
            if (ext === '.png' || ext === '.jpg' || ext === '.jpge') {
              list.push(
                Builder(FileInfoDto)
                  .filename(file.name)
                  .originalname(file.name)
                  .path(path.join(filePath, file.name))
                  .build(),
              );
            }
          }
        });
      return list;
    } catch (e) {
      this.logger.error(e.message);
      return new throws(e.message);
    }
  }
```
- 해당경로의 파일을 읽어 directory이면 재귀함수로 자기 자신을 호출하며 directory가 아니라면 파일 확장자를 통해 특정 파일을의 정보를 클래스에 담아 list를 반환한다.
