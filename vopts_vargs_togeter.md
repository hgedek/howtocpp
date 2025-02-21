# use VA_OPT and VA_ARGS together

in c macros; __VA_ARGS__ used to pass `...` to macro definition

```c++
#define FUNC(...) func(__VA_ARGS__)
```

if we need some optionals like `,` we can use `__VA_OPT__(,)` to place them

> If __VA_OPT__ is not used, pack will not be expanded correctly

```c++

#define FUNC(X, ...) func(X, __VA_ARGS__)o

FUNC(1,2,3) => func(1,2,3)
FUNC(1) => failed

#define FUNC(X, ...) func(X _VA_OPT__(,) __VA_ARGS__)

FUNC(1,2,3) => func(1,2,3)
FUNC(1) => func(1)

```

```c++
#define PRINT(...) printf(__VA_ARGS__)

#define LOG(format, ...) printf(format __VA_OPT__(, ) __VA_ARGS__)

#define DEBUG_LOG(format, ...)                                                 \
    printf("DEBUG: " format __VA_OPT__(, ) __VA_ARGS__)

int main() {
    PRINT("%s %s\n", "hakan", "gedek");
    PRINT("%d %d\n", 1, 2);
    PRINT("");

    LOG("%s %s\n", "hakan", "gedek");
    LOG("%d %d\n", 1, 2);
    LOG("");

    DEBUG_LOG("%s %s\n", "hakan", "gedek");
    DEBUG_LOG("%d %d\n", 1, 2);
    DEBUG_LOG("");
}

```