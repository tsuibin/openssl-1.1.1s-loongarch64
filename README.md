# OpenSSL 1.1.1s on loongarch64

## 结论：官方已支持，零补丁

OpenSSL 1.1.1s 的 `Configurations/10-main.conf` 里**自带 `linux64-loongarch64` target**（第 766 行附近），直接可用。

## 构建

    tar xzf openssl-OpenSSL_1_1_1s.tar.gz
    cd openssl-OpenSSL_1_1_1s
    ./Configure --prefix=$HOME/doris-port/installed --with-rand-seed=devrandom -shared linux64-loongarch64
    make -j4
    make install_sw

耗时约 2 分钟。

## 验证

    $ ./apps/openssl version -a
    OpenSSL 1.1.1s  1 Nov 2022
    platform: linux64-loongarch64

    $ echo -n hello | ./apps/openssl dgst -sha256
    (stdin)= 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824   # 正确

## 注意

- 安装到 `installed/lib`（不是 `lib64`），后续链接时要同时给 `-L.../lib -L.../lib64`。
- Doris 的 brpc 依赖它；动态库 `libcrypto.so.1.1` / `libssl.so.1.1` 运行时需要 `LD_LIBRARY_PATH`。
