# define singleton pattern in multiple ways

there are multiple ways to define singleton pattern in c++.

> local static variables are thread-safe.
> if you need; you can wrap creation with `lock_guard<mutex>` or you can use atomics

```c++
// using static - standard
struct Manager {
  private:
    std::tuple<int, float> m_Data;

    template <class... Ts>
    constexpr Manager(Ts... args) : m_Data(std::forward<Ts>(args)...) {}

  public:
    template <class... Ts>
        requires std::same_as<std::tuple<Ts...>, decltype(m_Data)>
    static Manager create(Ts... args) {
        static Manager inst{std::forward<Ts>(args)...};
        return inst;
    }
}

```

```c++
// using shared_ptr
struct manager {
    private:
    std::tuple<int, float> m_Data;

    template <class...Ts>
    constexpr Manager(Ts...args): m_Data(std::forward<Ts>(args)...){}

    public:

    template <class...Ts>
        requires std::same_as<std::tuple<Ts...>, decltype(m_Data)> 
    static std::shared_ptr<Manager> create(Ts...args) {
        static std::shared_ptr<Manager> inst{std::forward<Ts>(args)...};
        return inst;
    }
};
```

> If you want you can use define your instance as atomic too. 