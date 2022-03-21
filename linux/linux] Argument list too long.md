## Argument list too long 
- 리눅스에서 cp, mv, rm 등 커멘드 실행시 파일 개수가 너무 많으면 매개 변수 개수 제한 에러가 발생.

```bash
cp -r crop_dir/*_50_* /home/hsnam/data/50
```

- 특정 파일 이름을 가지는 파일을 복사 할때 파일 갯수가 많으면 매개 변수 개수 제한 에러 발생 


```bask
find . -name "*_50_*" -type f -exec cp {} /home/hsnam/data/50 \;
```

- 위와 같이 cp를 변경하면 Argument list too long 을 해결 할 수 있다.
