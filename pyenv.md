# pyenv

## macOS Big sur와 pyenv install 에러

> https://github.com/pyenv/pyenv/issues/1643

## pyenv install error

- pyenv install 3.6.4

```
Last 10 log lines:
    struct sf_hdtr sf;
           ^
./Modules/posixmodule.c:8210:15: error: implicit declaration of function 'sendfile' is invalid in C99 [-Werror,-Wimplicit-function-declaration]
        ret = sendfile(in, out, offset, &sbytes, &sf, flags);
```

- 해결

```
$ brew install zlib
$ export LDFLAGS="-L/usr/local/opt/zlib/lib"
$ export CPPFLAGS="-I/usr/local/opt/zlib/include"

brew info openssl

If you need to have openssl@1.1 first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"' >> ~/.zshrc

For compilers to find openssl@1.1 you may need to set:
  export LDFLAGS="-L/usr/local/opt/openssl@1.1/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl@1.1/include"

For pkg-config to find openssl@1.1 you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/openssl@1.1/lib/pkgconfig"

$ export LDFLAGS="-L/usr/local/opt/readline/lib -L/usr/local/opt/openssl@1.1/lib -L/usr/local/opt/zlib/lib"




- 해결되니 다음 에러가 등장했다.
```

Last 10 log lines:
make: **_ [Parser/pgen.o] Error 1
clang: error: no input files
clang: error: no input files
make: _** [Parser/tokenizer.o] Error 1
make: **_ [Parser/parsetok.o] Error 1
make: _** [Parser/myreadline.o] Error 1
clang: error: no input files
clang: error: no input files
make: **_ [Objects/abstract.o] Error 1
make: _** [Objects/accu.o] Error 1

```



- 해결책

```

macOS Big Sur: Version 11.1

Installing build dependencies

brew install zlib
brew install sqlite
brew install bzip2
brew install libiconv
brew install libzip
Installing Python 3.4, 3.5 and 3.6
LDFLAGS="-L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install --patch 3.4.10 < <(curl -sSL https://github.com/python/cpython/commit/8ea6353.patch\?full_index\=1)

LDFLAGS="-L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install --patch 3.5.10 < <(curl -sSL https://github.com/python/cpython/commit/8ea6353.patch\?full_index\=1)

LDFLAGS="-L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install --patch 3.6.12 < <(curl -sSL https://github.com/python/cpython/commit/8ea6353.patch\?full_index\=1)

Installing Python 3.7 and above
LDFLAGS="-L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install 3.7.9

LDFLAGS="-L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install 3.8.6

LDFLAGS="-L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install 3.9.0

```

주의할 점은 세팅한다음 `pyenv install 3.6.5` 이런식으로 명령하면, 동작하지 않는다 ㅎㅎ
```
