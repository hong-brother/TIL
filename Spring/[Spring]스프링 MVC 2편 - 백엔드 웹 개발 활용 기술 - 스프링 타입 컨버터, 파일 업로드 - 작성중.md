# 스프링 타입 컨버터
## 

# 파일 업로드
- 일반적으로 사용하는 HTML Form을 통한 파일 업로드를 이해하려면 먼저 폼을 전송하는 다음 두가지 방식의 차이를 이해 해야 한다.

## application/x-www-form-urlencoded
![](https://velog.velcdn.com/images/hong-brother/post/3b3b1ca0-d589-4e1c-bd2b-6708cfb598eb/image.png)

- applicaction/x-www-form-urlencoded 방식은 HTML 폼 데이터를 서버로 전송하는 가장 기본적인 방법이다.
- 첨부 파일과 이름과 나이도 같이 전송해야 하기 때문에 문제가 발생한다. 
- 이를 해결하기 위해서는 HTTP는 multipart/form-data라는 전송 방식을 제공한다.

![](https://velog.velcdn.com/images/hong-brother/post/d311f110-4484-439f-b2b8-7333a92106a7/image.png)

- multipart/form-data 방식은 다른 종류의 여러 파일과 폼의 내용을 함께 전송 할 수 있다.
- Content-Disposition이라는 항목별 헤더가 추가되어 있고 여기서 부가 정보가 있다.
