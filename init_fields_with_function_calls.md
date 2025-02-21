# initialize fields with functions

struct/class/union has an order of initialization of fields in the order of they are defined.

- we can initialize them like any other variables
- depending on decl of thme { static, constexpr } we can use better inits 
- we can pass them to next init functions

```c++

auto getId() {
    static int counter = 101;
    return counter++;
}

auto getName(int id) { return std::format("Object: {}", id); }

auto getFullName(std::string const &name, uintptr_t addr) {
    return std::format("{} {}", name, addr);
}

struct Object {
    int id = getId();
    std::string name = getName(id);
    std::string fullName = getFullName(name, reinterpret_cast<uintptr_t>(this));
};

void print(Object const &obj) {
    std::cout << std::format("Id: {} Name: {} FullName: {}", obj.id, obj.name,
                             obj.fullName)
              << std::endl;
}

int main() 
{
    Object obs[] = {{}, {}, {}};
    print(obs[0]);
    print(obs[1]);
    print(obs[2]);
}

```

