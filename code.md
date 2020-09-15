## 1.有关内存的GetMemory（）函数
例1：
```C
    void GetMemory1(char *p)
    {
        P = (char*)malloc();
    }

    void Test()
    {
        char *str = NULL;
        GetMemory(str);
        strcpy(str,"Crash");
        printf("str = %s",str);
    }
/*
问题分析：
    1.GetMemory不能传递动态内存，导致Test函数中的str一直都是NULL
    2.strcpy(str,"Crash");往一个NULL的指针复制数据会崩溃
    3.每执行一次GetMemory1就会泄露一块内存，因为没有用free释放内存。
*/
```
例2：
```C
    void *GetMemory2(void)
    {
        char P[] = "Hello World!";
        return p;
    }

    void Test()
    {
        char *str = NULL;
        str = GetMemory2(); 
        printf(str);
    }
/*
问题分析：
    可能乱码。
    字符数组p存在于栈空间，是局部变量，函数返回后，内存空间被释放，因此输出无效值
    因为GetMemory2返回的是指向“栈内存”的指针，该指针的地址不是NULL，但是其原来的内容已经被清除了，新内容不可知
*/
```
例3:
```C
char *GetMemory3(void)
{ 
    return "hello world";
    /*
    另一种写法也可以
    char p[] = "hello world";  
    return p;
    */
}
void Test3(void)
{
    char *str = NULL;
    str = GetMemory3(); 
    printf(str);
}
//Test3 中打印hello world，因为返回常量区，而且并没有被修改过。
}
```
例4:
```C
void GetMemory4(char **p, int num)
{
    *p = (char *)malloc(num);
}
void Test4(void)
{
    char *str = NULL;
    GetMemory4(&str, 100);
    strcpy(str, "hello"); 
    printf(str); 
}
```
例5:
```C
void GetMemory6(void) {
	char *str = (char*)malloc(100);
	strcpy(str, "hello");
	free(str);
        //str = NULL，加上这句程序才不会有野指针
        if (str != NULL) {
		strcpy(str, "world");
		printf(str);
	}
}
 
void main(){  
	GetMemory6();
}
/*
结果：能够输出world，但程序存在问题。
分析：程序出现了野指针。

     野指针只会出现在像C和C++这种没有自动内存垃圾回收功能的高级语言中, 所以java或c#肯定不会有野指针的概念. 当我们用malloc为一个指针分配一个空间后, 用完这个指针，把它free掉，但是没有让这个指针指向NULL或某一个特定的空间。如上面程序一样，将str进行free后，只是释放了指针所指的内存，但指针并没有释放掉，此时指针所指的是垃圾内存；这样的话，if语句永为真，if判断无效。delete也存在同样的问题。

      防止产生野指针：（1）指针变量一定要初始化为NULL，因为任何指针变量刚被创建时不会自动成为NULL指针，它的缺省值是随机的。（2）当free或delete后，将指针指向NULL。通常判断一个指针是否合法，都是使用if语句测试该指针是否为NULL
*/
```
