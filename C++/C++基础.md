1. #### 函数重载 (overreload)

   指的是函数名相同，函数个数不同，或者函数参数类型不同，或者函数参数顺序不同。函数重载指的是函数的参数，和返回值无关。在C++中函数重载之所以不会和像C语言中产生冲突，原因是编译器会根据函数的名字和函数的参数类型产生不同的函数，以此来辨别不同的函数，但是在不同的编译器产生的规则是不一样的

   ```c++
   //生成的函数名称为overreloadTest_i
   void overreloadTest(int num){
       cout << num << "int";
   }
   //生成的函数名称为overreloadTest_d
   void overreloadTest(double num){
       cout << num << "double";
   }
   int main(int argc, const char * argv[]) {
       overreloadTest(3l);
       overreloadTest(2.0);
       return 0;
   }
   ```

   > 在swift中也是支持函数重载的，不同的是swift中可以支持通过泛型来进行函数重载，当你在用确定类型来调用这个函数的时候，编译器会自动生成一个和你所调用类型相同的函数
   >
   > ```swift
   > func swiftTest<T>(_ num : T){
   >     print("swiftTest_T")
   > }
   > 
   > //func swiftTest(_ num : Int){
   > //    print("swiftTest_Int")
   > //}
   > 
   > func swiftTest(_ num : Double){
   >     print("swiftTest_Double")
   > }
   > 
   > //当func swiftTest(_ num : Int){}注释掉的话就会走 func swiftTest<T>(_ num : T)函数
   > swiftTest(1)
   > print("Hello, World!")
   > 
   > 
   > ```
   >
   > 

2. #### extern "C"

   在C++工程中调用C语言函数的时候，需要添加`extern C`，因为在C++中编译方式的不同，导致生成的函数名称不一样，出现找不到C函数的错误，加上`extern C`可以让c函数在C++的环境中编译

   使用`__cplusplus` 可以判断是在C环境中还是C++环境中

   使用extern 的两种方式

   ```c++
   方式一：
   using namespace std;
   extern "C" void test(int a);
   int main(int argc, const char * argv[]) {
       test(1);
       return 0;
   }
   void test(int a){
       cout << a << "int";
   }
   
   方式二：
   #ifdef __cplusplus
       extern "C" {
   #endif
        int add(int a, int b);
   #ifdef __cplusplus
       }
   #endif
   
   #include "add.h"
   int add(int a, int b){
       return a + b;
   }
   
   ```

   

3. #### 默认参数

   C++函数允许设置参数，默认参数只能从右到左的顺序，当函数声明和实现都有的时候，默认参数只能放到声明中

   ```c++
   void add(a:int = 2,b : int = 3){
     cout << "a is" << a << endl;
     cout << "b is" << b << endl;
   }
   
   int main() {
     add();
     return 0;
   }
   ```


   在C++中如果有两个相同函数有参数省略会产生二义性，而swift中不会，且会选择没有默认参数的函数为实现

   ```c++
   //产生二义性
   void add(a:int,b : int = 3){
     cout << "a is" << a << endl;
     cout << "b is" << b << endl;
   }
   
   void add(a:int){
     cout << "a is" << a << endl;
   }
   
   int main() {
     add();
     return 0;
   }
   ```

   在swift中 ：

   ```swift
   func add(_ a : Int )  {
       print("add")
   }
   
   func add(_ a : Int , _ b : Int = 0)  {
       print("add_b")
   }
   
   print(add(1))
   //add
   ```

   

