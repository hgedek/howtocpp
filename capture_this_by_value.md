# capture this pointer by value

we can capture this as by-ref normally but when we need to create a copy from it ?

```c++

struct Type
{
    void by_ref() {
        auto f = [this]{
            std::cout << m_Id << std::endl; 
        };
        f();
    }

    void by_value() {
        auto f = [*this]{
            std::cout << m_Id << std::endl;
        };
        f();
    }

    void by_copy_value() {
        auto f = [thisCopy = *this] {
            std::cout << thisCopy.m_Id << std::endl;
        };
    }

    int m_Id;
};

int main() {
    Type t{101};
    t.by_ref();
    t.by_value();
    t.by_copy_value();
}

```