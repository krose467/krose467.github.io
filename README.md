一、c++11新特性
1.using给数据类型起别名
using 变量名 = 数据类型
类比于c里面的typedef：typedef 数据类型 变量名

2.array<数据类型， 大小> arr{};
相当于c中的定长数组，相比vector变长数组来说，在初始化的时候就确定大小，更节省内存，大小需要输入常数， 用arr.fill()函数来进行初始化。

3.mt19937 随机数算法
之前的随机数算法rand()%a + b :相当于生成一个从b开始，大小为b + a - 1范围的随机数，而且需要加上srand(unsigned time(NULL))来初始化随机种子，mt19937算法可以生成2^19937范围内的随机数，相比之前的(大概在2^16左右)随机数算法，更加强大
unsigned seed = random_device{}(); // 初始化随机种子
mt19937 mt(seed); //创建随机数对象
uniform_int_distribution<int> dis(起始范围，终止范围); //给定随机数范围；
cout << dis(mt) << endl;  //生成随机数
  
4.右值引用&&
①右值引用：一个不能被取地址或者没有名字的表达式，函数，变量等称作右值，可以被取地址，有名字的称作左值。
右值：1， a + b，函数返回值这类临时变量（将亡量）等等
左值：int a = 10，a就是左值
注意：int&& b = 10,此时b虽然写的是右值形式，但依然是左值
②右值引用的作用：
1.直观作用：为临时变量续命，因为右值在表达式结束后就消亡了，如果想继续使用右值，就会动用昂贵的拷贝构造函数。
2.做转移语义作用：转移语义可以将资源（堆， 系统对象等）从一个对象（临时对象）转移到另一个对象，这样能减少不必要的临时对象的创建，拷贝和销毁，能大幅提高c+=应用程序的性能。
3.转移构造函数和重载转移赋值运算符：我们可以定义拷贝构造和赋值函数。要实现转移语义，需要定义转移构造函数，还可以定义转移赋值运算符。对于右值的拷贝和赋值操作调用这个两个构造函数。
比如在自己定义的字符串类中，当我们要创建一个新的对象时，会有这样的写法：
MyString str;
str("sefsdf"); //对于这个表达式来说就是临时对象，虽然是临时的，但程序仍然调用了拷贝构造，造成了没有必要的资源申请和释放操作。如果可以直接使用临时对象已经申请的资源，既节省资源，也节省时间。这正是转移语义的目的。
4.完美转发作用：写一个接受任意实参的函数模板，并转发到其他函数，目标函数会受到与转发函数完全相同的实参。
③move()函数
可以将一个左值无条件的转化为右值
int&& a  = 10;
int && b = move(a);
④forward()函数：实现完美转发
  
5.auto类型和decltype类型
auto:自动类型推导，一般是变量类型的推导
decltype:表达式自动类型推导，一般用在复杂计算或者返回值类型推导上，不会真的去求解表达式的值，只是在编译期推导
三种结构：
①decltype + 变量
声明了一个变量， 之后作用于其他变量时可以直接得到变量的类型。
②decltyte + 表达式
decltype会返回表达式结果对应的类型，结果是左值的表达式得到类型的引用，结果是右值的表达式得到类型
③decltype + 函数名
decltype会返回函数类型，一般用于函数指针：decltype(test)* t = test; t();
  
6.nullptr
c++11引入nullptr来区分空指针和0，一般c++会将NULL和0视为同一种东西，这取决于编译器如何定义NULL，有的定义为(void*)0，有的定义为0，为了防止c++重载时发生混乱，在需要使用NULL时，用nullptr代替。
  
7.范围for循环
for (auto&c : nums) {}
  
8.初始化列表
用花括号来进行初始化称为初始化列表
如：int x = {0};
int x{0};
class AA{
public:
AA(int age):m_age(age){}
private:
int m_age;
};
  
9.lambda表达式
[](){} | [=](){} | [&](){} 
auto func = [](int a)->decltype(a){return int};

10.并发

11.constexpr将运算尽量放在编译阶段， 而不是运行阶段。
constexpr表示常量，const表示只读，constecpr只能定义编译期常量，而const可以定义编译期和运行期的常量。将一个函数或者变量标记为constexpr，则它也是const的，反之不成立。
constexpr + 变量：必须使用常量初始化
constexpr + 函数：有且只有一条返回语句，能够在编译期计算出来，否则会视为普通函数
constexpr + 构造函数：所有成员变量必须初始化列表来初始化，对象调用的成员函数必须用constexpr修饰
一般系统会将constexpr修饰为内联函数
  
12.带作用域的枚举 enum class A : int {};
不带作用域的枚举会自动转化为int类型，不同的枚举可以相互比较，这样会带来潜在bug

13.explicit专用于修饰构造函数，表示只能显示构造，不可以被隐式转换
显示构造：int a(10);
隐式构造：int a = 10;
  
14.default
当在类中写入有参构造后，无参构造被覆盖，如果此时想构建无参构造的对象，可以在类中加入 A() = default;
  
15.delete：如果想禁止对象的拷贝与赋值，可以使用delete修饰
A(const& A) = delete;
&A operator(const& A)  = delete;
delete在c++11中很常用，unique_ptr就是通过delete修饰来禁止对象拷贝。





二、STL
1.next_permutation(.begin(), .end())：全排列算法
会在原本输入的数据的基础上返回比当前数据刚好大一个的全排列数据，如果这个数据存在，就返回true，否则返回false。


三、c++一些知识点

1.c++将浮点型数num保留n位小数
ostringstream os;
os.persion(n);
os << fixed << num;
如果要返回的是字符串型就num.str()

2.字符串转化为整型
①stoi()，stol()....
②stringstream ss;
string s = "1235";
int n;
ss << s;
ss >> n;
此时n就是1235

3.整型转化为字符型
①to_string()
②stringstream ss;
string s;
int n = 354;
ss << n;
ss >> s;
此时s就是"354"

4.用stringstream + getline()分割字符串
string&& s = "sdfsfd/sdfsdf/dfgfdg/drgdfg/cbcfhf/dfdb";
stringstream ss(s);
string word = "";
while (getline(ss, word, '/') {
cout << word << endl;
} 

5.C与C++的内存分配方式
①：从静态存储区分配：内存在程序编译好的时候就已经分配好，这块内存在程序的整个运行期间都存在，如全局变量，static变量。
②：从栈区分配：在执行程序时，函数内局部变量的存储单元都可以在栈上创建，程序执行结束这些存储单元自动被释放。栈内存分配效率高，但空间有限。
③：从堆区分配：程序在运行时使用new或者malloc申请任意大小的内存，在结束使用时需要手动释放。


四、c++中用c库函数

1.memcpy(void*dest, void*src, size_t n);
用来拷贝src所指的内存内容前n个字节到dest所指的内存地址上，遇到'/0'不会结束（src和dest所指向的内存区域不可重叠）。
memcpy()函数 // 内存不重叠或者dst在左，src在右覆盖了部分dst
void* my_memcpy(void* dst, const void* src, size_t n)
{
    char *tmp = (char*)dst;
    char *s_src = (char*)src;
 
    while(n--) {
        *tmp++ = *s_src++;
    }
    return dst;
}

2.内存安全的memcpy()函数：memmove()函数
用来拷贝src所指的内存内容前n个字节到dest所指的内存地址上，遇到'/0'不会结束（src和dest所指向的内存区域可重叠）。
void* my_memmove(void* dst, const void* src, size_t n)
{
    char* s_dst;
    char* s_src;
    s_dst = (char*)dst;
    s_src = (char*)src;
    if(s_dst>s_src && (s_src+n>s_dst)) {      //-------------------------第二种内存覆盖的情形。
        s_dst = s_dst+n-1;
        s_src = s_src+n-1;
        while(n--) {
            *s_dst-- = *s_src--;
        }
    }else {
        while(n--) {
            *s_dst++ = *s_src++;
        }
    }
    return dst;
}

3.sprintf(char*buff, const char*format, argument)：将各种数据类型转化为字符串型
char buff[64];
int a = 1324;
sprintf(buff, "%d", a);
sprintf(buff, "%d:%s", a, "hello world");
此时输出buff就是char*类型






