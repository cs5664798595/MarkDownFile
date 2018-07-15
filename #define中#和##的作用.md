# #define中#和##的作用

* > #### #的功能是将其后面的宏参数进行字符串化操作（Stringfication），简单说就是在对它所引用的宏变量通过替换后在其左右各加上一个双引号
* > #### ##被称为连接符（concatenator），用来将两个Token连接为一个Token，##符是把传递过来的参数当成字符串进行替代。

```
#define f(a,b) a##b 
 #define d(a) #a 
 #define s(a) d(a) 
 void main( void ) 
 { 
     puts(d(f(a,b))); 
     puts(s(f(a,b))); 
 } 
```

 输出结果: 

```
 f(a,b) 
 ab
```

 分析：  ##把两个符号连起来 
     #a指把a当成符号，就是把#后面的看成字符串

 \# 和 ## 操作符是和#define宏使用的. 使用# 使在#后的首个参数返回为一个带引号的字符串. 例如, 命令 

     ```
#define to_string( s ) # s 
     ```

 将会使编译器把以下命令 

    ```
 cout < < to_string( Hello World! ) < < endl; 
    ```

 理解为 

     ```
cout < < "Hello World!" < < endl; 
     ```

 使用##连结##前后的内容. 例如, 命令 

```
   #define concatenate( x, y ) x ## y 
     ... 
     int xy = 10; 
     ... 
```

 将会使编译器把 

     ```
cout < < concatenate( x, y ) < < endl; 
     ```

 解释为 

     ``` 
cout < < xy < < endl; 
     ```

 理所当然,将会在标准输出处显示'10'.

  

* puts(d(f(a,b)));  ----> 因为d宏中的参数是另外一个宏，且带##，所以作为参数的宏不展开，相当于 

    ``` 
    puts(#f(a,b));----->puts("f(a,b)");  
    ```

* puts(s(f(a,b))); ----> 因为s宏中的参数是另外一个宏，但不带##，所以作为参数的宏先展开，相当于 

       ``` puts(s(ab));----->puts(d(ab));---->puts(#ab);---->puts("ab");
puts(s(ab));----->puts(d(ab));---->puts(#ab);---->puts("ab");
       ```



  