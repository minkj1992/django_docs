# pytest django
> https://pytest-django.readthedocs.io/en/latest


## config (3)

- 우선순위
  1. The command line option `--ds`
  2. The environment variable DJANGO_SETTINGS_MODULE
  3. The DJANGO_SETTINGS_MODULE option in the configuration file - pytest.ini, or other file that Pytest finds such as tox.ini

## Python path

디폴트로 자동으로 path를 찾아주지만, 프로젝트 구조를 커스터마이징 했다면[explicitly set path](https://pytest-django.readthedocs.io/en/latest/managing_python_path.html#managing-the-python-path-explicitly) 참조

> By default, pytest-django tries to find Django projects by automatically looking for the project’s manage.py file and adding its directory to the Python path.

## 사용

```bash
# Django의 기본 test cmd
$ manage.py test # app안에 tests 모듈을 찾아 실행, 느림

# pytest cmd
$ py.test
$ py.test --nomigrations
$ py.test apps/fantem/tests/views/test_home.py # 특정 파일 실행
```


## model_bakery
> https://github.com/model-bakers/model_bakery

- model_mommy 의 후속작
- factory_boy와 비교 대상 (https://github.com/FactoryBoy/factory_boy)


## 

RequestFactory returns a request, while Client returns a response

View 클래스를 단위 테스트하는 경우에는 Request Factory를 사용하고, 예를 들어 전체 요청 / 응답주기를 테스트하려면 클라이언트를 사용합니다