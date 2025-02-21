# create decorator with lambda

key feature of a lambda is capturing around. using this feature; we can create a creator / decorator easily.

let create a printer...

```c++

void print(auto arg) { std::cout << arg << ' '; }
void print(auto arg, auto... args) {
    print(arg);
    print(args...);
}


// decorating inner creator with outer params
auto create_printer(auto...args) { // args: shared by inner creator
    return [=](auto arg) {
        print(args...); // 
        print(arg);

        std::cout << '\n';
    };
}

// 3 layered decorator... we can share first and second layers between inner typed objects
auto create_more_printer(auto ...args) {
    return [=](auto arg0) {
        return [=](auto arg1) {
            print(args...);
            print(arg0);
            print(arg1);

            std::cout << '\n';
        };
    };
}

int main() {
    // first layer is fixed and captured by inner of decorator
    auto printer = create_printer(1,2,3);

    // we can use them with different args now
    printer("hakan"); 
    printer("gedek");
}

```

> when capturing I used by-value (=) because the captured args lifetimes maybe shorter than decorated instance. If by-ref(&) was used then it would create an UB (undefined behaviour) so when capturing with lambda be careful with lifetimes of the args.
