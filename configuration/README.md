# Configuration example

This program is a copy of configuration example provided by FastDDS to illustrate a way to use exclusive ownership.

### Build and run publisher with exclusive ownership

```shell
cmake .
cmake --build .
./configuration publisher -o
```

### Build and run subscriber with exclusive ownership

```shell
cmake .
cmake --build .
./configuration subscriber --ownership-strength 20
```
