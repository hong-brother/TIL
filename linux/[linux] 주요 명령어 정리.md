- 특정 파일 용량확인
```bash
du -csh *_30_*
```
- 특정 파일 개수 확인
```bash
find *_40_* -type f | wc -l
```
- 특정 파일 확인
```bash
find . "*.JPG" -type f | wc -l
find dirname -type f -name "*_30_*" -ls | awk '{ result += $7 } END { print result }'
```
