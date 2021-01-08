# pip

## Update by requirements.txt

> requirements.txt를 기준으로 pip install

```bash
pip install --upgrade pip
pip install -r requirements.txt --upgrade
```

## Remove

```bash
pip uninstall -r unwanted-requirements.txt
```

## error: command 'clang' failed with exit status 1

- python 3.6.8
- librabbitmq pip install 하는 동작에서 에러가 발생했다.
- 이전의 macos mysqlclient 문제와 비슷한데, 이번에는 librabbitmq이다.

```
ERROR: Command errored out with exit status 1: /Users/kakao/.pyenv/versions/3.6.8/envs/koin/bin/python3.6 -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/private/var/folders/ry/fj056h9d2y16d35_cj5hc8p80000gn/T/pip-install-sjlbf1w8/librabbitmq_e8deef37dc37418f8e163e2004d1f945/setup.py'"'"'; __file__='"'"'/private/var/folders/ry/fj056h9d2y16d35_cj5hc8p80000gn/T/pip-install-sjlbf1w8/librabbitmq_e8deef37dc37418f8e163e2004d1f945/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record /private/var/folders/ry/fj056h9d2y16d35_cj5hc8p80000gn/T/pip-record-7374y8qs/install-record.txt --single-version-externally-managed --compile --install-headers /Users/kakao/.pyenv/versions/3.6.8/envs/koin/include/site/python3.6/librabbitmq Check the logs for full command output.
```

- 시도 1 (x)
  - pip install thriftpy==0.3.9, Cython
- 시도2 (x)
  - brew install autoconf automake pkg-config libtool
  - pip install librabbitmq==2.0.0
- 시도3

  - brew reinstall pkg-config
  - pip install librabbitmq==2.0.0

- 찾아보니 Big Sur에서 사용하는 Xcode cli 버전 12와 pip install librabbitmq가 호환 되지 않는 문제가 있습니다. (https://github.com/celery/librabbitmq/issues/123#issuecomment-683665386) 이를 위해서 방법은 `librabbitmq`대신 `py-amqp`를 사용하는 것
