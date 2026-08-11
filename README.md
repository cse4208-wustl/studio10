# Studio 10

## Dynamic Memory Management

In this studio, you will explore dynamic memory management in modern C++. The exercises compare raw `new` and `delete` usage with safer standard library facilities such as `shared_ptr`, `weak_ptr`, `unique_ptr`, and `allocator`, while highlighting the lifetime and ownership behavior of each approach.

## Collaboration

You may complete this studio individually or in a small group.

## Reference

If you need a refresher on the environment setup steps from the previous studios, see [Studio 0](https://github.com/cse4208-wustl/studio0).

## Exercises

Record your answers in `ANSWERS.md` as you work. Include the names of everyone who worked on the studio in your first answer, and number your responses so they are easy to match to the exercises.

1. List the names of the people who worked together on this studio.

2. SSH into `shell.cec.wustl.edu` using your WUSTL Key credentials, then use `qlogin` to connect to one of the Linux Lab machines and confirm that the version of `g++` there is correct, as you did in [Studio 0](https://github.com/cse4208-wustl/studio0).

   Clone your `studio10` repo and work inside that cloned directory.

   The repo already includes a starter `studio10.cpp` and a `Makefile`. Update them as needed so the repo builds an executable named `studio10`.

   Add a header file and source file for this class that has:

   - a `static` member variable of type `size_t` initialized to `0` that tracks how many objects of the class have been constructed
   - a non-static member variable of type `size_t` that will be used as a numeric identifier for this object.
   - a default constructor that initializes the non-static member variable with the current value of the static member variable, increments the static member variable, and prints a message indicating that the default constructor was called along with the member value and object address
   - a copy constructor that initializes the non-static member variable from the source object's corresponding member value, increments the static member variable, and prints a message indicating that the copy constructor was called along with the member value and object address
   - a destructor that prints a message indicating that it was called along with the member value and object address, without modifying the static member variable

   In `main`, declare one object of that class type on the stack, copy-construct another object of the same type from it, and return a descriptively named symbol whose value is `0` to indicate success.

   Build and run your program. In your answers, show:

   - the output your program produced
   - the code for your program's `main` function

3. Remove the contents of `main` except for the statement at the end that returns a success value.

   In `main`, use the array form of `new` to allocate a dynamic array of objects of your class type, store the returned address in a plain C++ pointer, and then use the array form of `delete` on that pointer to destroy the array of objects. Build and run the program, and observe the output it produces.

   Then replace the array form of `delete` with plain `delete` on the pointer. Build and run the program again.

   In your answers, briefly explain what happens and what problem this reveals when the non-array form of `delete` is used to free memory that was allocated with the array form of `new`.

4. Remove the contents of `main` except for the statement at the end that returns a success value.

   In `main`, declare a `shared_ptr` parameterized with your class type and initialize it with `make_shared` taking no arguments. Declare another `shared_ptr` parameterized with your class type and initialize it with `make_shared` using a dereference of the first `shared_ptr` as its only argument.

   Build and run your program, and confirm that it produced the same output as in the first exercise.

   In your answers, show the statements that declare and initialize the `shared_ptr` variables in `main`.

5. In the header and source files for your class, declare and define a public member function that prints a message indicating that it was called along with the non-static member value and the object's address, without modifying the static member variable.

   Just below the `shared_ptr` declarations in `main`, declare a `weak_ptr` and initialize it with the first `shared_ptr`. Then declare another `shared_ptr` and initialize it with a call to the `lock` member function of the `weak_ptr`.

   If that `shared_ptr` is equal to `nullptr`, print a message saying that the `weak_ptr` no longer points to a valid object. Otherwise:

   1. use that `shared_ptr` to call the public member function on the object to which it points
   2. assign `nullptr` to that `shared_ptr` to break its association with the object

   Build and run your program. In your answers, show the output that your program produced.

6. Just below where you declared and used the `weak_ptr`, add a statement that assigns the second `shared_ptr` you declared at the top of `main` to the first `shared_ptr` you declared at the top of `main`.

   Before and after that statement, print messages indicating that your code is about to make that assignment and then has completed it, so you can see what happens and when as a result.

   Just below that, assign the third `shared_ptr` the result of a call to the `lock` member function of the `weak_ptr`. If that `shared_ptr` is equal to `nullptr`, print a message saying that the `weak_ptr` no longer points to a valid object. Otherwise use that `shared_ptr` to call the public member function on the object to which it points.

   Build and run your program. In your answers:

   - show the output your program produced
   - explain briefly why the `shared_ptr` reference-counting semantics appear to be working correctly based on that output

7. Remove the contents of `main` except for the statement at the end that returns a success value.

    In the source file where you define `main`, define a function that takes no parameters, declares a `unique_ptr` to your class type, initializes it with the address of an object of your class type returned by `new`, and returns that `unique_ptr` by value.

    In `main`, declare a `unique_ptr` to your class type and initialize it with a call to that function. Declare a second `unique_ptr` of the same type, and attempt to initialize it from the first `unique_ptr`.

    Try to build the program. In your answers:
  - show the compiler error you get
  - briefly explain, in your own words, what that error tells you about how `unique_ptr` treats copying

8. Comment out the failed copy-initialization from the previous exercise so the program builds again.

    In the source file where you define `main`, define a function that takes a reference to a `unique_ptr` to your class type and uses it to invoke the public member function of the object to which it points.

    In `main`, just below the commented-out copy-initialization, declare a second `unique_ptr` of the same type and initialize it by moving from the first `unique_ptr` using `std::move`.

    Pass the second `unique_ptr` into the function you just defined to invoke the public member function through it, then attempt to do the same using the first `unique_ptr`.

    Build and run your program. In your answers:
  - show the output your program produced
  - explain what happened when you tried to use the first `unique_ptr` after moving from it, and why

9. Remove the contents of `main` except for the statement at the end that returns a success value.

    In `main`, declare a `unique_ptr` to your class type inside a nested block (a pair of braces within `main`), initialized with the address of an object of your class type returned by `new`. Print a message immediately before and after the closing brace of that block.

    Build and run your program. In your answers:
  - show the output your program produced
  - explain what caused the destructor to run, and contrast this with the `delete` calls you had to write explicitly in Exercise 3

## Deliverables

Commit and push all modified and added files, including `ANSWERS.md`, to the repo.
