# HttpRequest, HttpResponse
> [참고](https://ssungkang.tistory.com/entry/Django-HttpRequest-HttpResponse)

## HttpRequest
> 클라이언트로 부터 들어오는 모든 요청을 wrap하는 클래스

- views.py `def method(request):`인 request가 해당 클래스이다.
- FBV에서는 request로, CBV에서는 self.request로 접근한다.

### HttpRequest's Attributes

method

request 요청의 종류를 나타냅니다. "GET" 또는 "POST" 로서 대문자로 저장이 됩니다. 다음과 같은 분기문을 통해서 들어온 요청에 종류에 따라서 다른 작업을 수행할 수 있습니다.

```python
def post_create(request):
if request.method == "POST":
    # 생략
else if request.method == "GET":
    # 생략
```

- GET: GET 인자 목록에 접근 할 수 있습니다. 자료형은 QueryDict 입니다.

- POST: POST 인자 목록에 접근 할 수 있습니다. 마찬가지로 자료형은 QueryDict 입니다. 이 때 file 에 대해서는 접근할 수 없습니다.

- FILES: POST 인자 중 파일에 한해서 접근이 가능합니다.

c.f) QueryDict 란?

python의 dict 과 유사한 자료형으로서 차이는 dict 은 Key 값이 중복 안되는데 비해 querydict 은 **중복을 허용합니다.**

## HttpResponse
> html 파일, 이미지 등 다양한 응답을 해줄 수 있습니다. 

FBV 기준으로 HttpResponse 가 없다면 오류를 발생합니다. 하나의 함수는 최소 하나의 HttpResponse 을 반환해야합니다. 이는 views 에서 검사하는게 아니라 middleware 에서 검사를 하게 됩니다.

`config/settings.py`

    MIDDLEWARE = [
        'django.middleware.security.SecurityMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.middleware.common.CommonMiddleware',
        'django.middleware.csrf.CsrfViewMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'django.contrib.messages.middleware.MessageMiddleware',
        'django.middleware.clickjacking.XFrameOptionsMiddleware',
    ]

middleware 는 다음과 같이 app을 감싸고 있으면서 request 가 들어올 때는 적절하게 가공해주고 response 가 나갈 때도 확인을 해줍니다. 여러 middleware 중 HttpResponse 의 반환 유무를 확인해주는 것도 있습니다.

### HttpResponse Usage

```python
# 방법 1
response = HttpResponse(
	"<div>Hello World!</div>"
)
# 방법 2
response = HttpResponse()
response.write("<div>Hello World!</div>")
```