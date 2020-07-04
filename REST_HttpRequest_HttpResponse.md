# Django REST에서의 REQ & RESP
> Django의 HttpResponse & HttpRequest처럼 request를 사용한다.

## Request
- request.POST  # Only handles form data.  Only works for 'POST' method.
- request.data  # Handles arbitrary data.  Works for 'POST', 'PUT' and 'PATCH' methods.

더 이상 지정된 콘텐츠 유형에 대한 요청 또는 응답을 명시 적으로 연결하지 않아도 됩니다.`request.data`로 들어오는 json요청을 처리 할 수 있으며, 추가로 다른 형식들도 처리 할 수 ​​있습니다. 마찬가지로 데이터가 포함 된 응답 객체를 반환하지만 REST 프레임 워크가 응답을 올바른 컨텐츠 유형으로 렌더링하도록 허용합니다.

## Response
> 내부적으로 `TemplateResponse`타입이며, Response()키워드를 사용해 wrap한다.

- 클라이언트가 요청한 타입에 맞게 변환해서 제공한다.

```python
return Response(data)  # Renders to content type as requested by the client.
```

## Status
뷰에서 숫자 HTTP 상태 코드를 사용한다고해서 항상 읽을 수있는 것은 아니며 오류 코드가 잘못되었는지 쉽게 알 수 없습니다. REST 프레임 워크는 각 상태 코드에 대한 더 명확한 식별자 제공을 합니다. 

예를 들면 `HTTP_400_BAD_REQUEST`에서 status모듈을 사용하는 것처럼, 숫자 식별자 보다 명확하게 의미를 전달합니다.

## Wrapping API views
> REST 프레임워크는 2가지 wrapping 모델을 제공한다.

- `@api_view` decorator: FBV
- `APIView class`: CBV

두가지 wrapping 모델은 다음과 같은 기능을 합니다.

- 상태 전달
  - `Response objects`에게 context를 첨부
- behaviour
  - return `405 Method Not Allowed responses`
  - `ParseError` exception when access `request.data`
