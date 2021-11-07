---
title: C++栈区和堆区
date: 2019-01-01 13:05:30
categories: C++
tags:
- C++
---

# C++栈区和堆区

 

# 1.栈区

- ## 局部变量 存放在栈区，栈取的数据在函数执行完之后自动释放

- ##  栈区的数据由编译器管理开启和释放

  ```C++
  int* func(int b)//形参也会存放在盏区
  {
      b=100;
  	int a = 10;
  	return &a;  //返回局部变量的地址
  }
  
  int main(){
      //利用指针来接回函数的返回值
      int *p = func(1)；
      cout<<*p<<endl;  //输出10  第一次净额已打印正确的数据，是因为编译器做了保留
      cout<<*p<<endl;  //乱码    第二次这个数据就不在保留了
  }
  ```

  





# 2.堆区

- ##  由程序员分配释放，若程序员不释放内存，程序结束的时候有操作系统回收

- ## 在C++中主要利用new在堆区开辟内存

- ## 堆区开辟的数据，由程序员手动开辟，手动释放，释放利用操作符delete

  ```C++
  int* func()
  {
      //利用new关键字 可以将数据开辟到遁去
      //指针本质也是局部变量，放在盏上，指针保存的数据是放在了堆区
      int* p = new int(10);
      return p;      
  }
  
  int main(){
      //在堆区开皮数据
      int *p = func();
  }
  ```

  

  ```c++
  //1.new的基本语法
  
  int* func()
  {
  	//在堆区创建整形数据
      //new返回是  该数据类型的指针
      int* p = new int(10);
      return p;
  }
  
  void test01()
  {
      int* p = func();
      count<< *p <<endl;
      count<< *p <<endl;
      count<< *p <<endl;
      //堆区的数据 由程序员管理开辟，程序员管理释放
      //如果想释放堆区的数据，利用关键字 delete
      
      delete p;//把数据的地址释放干净
      
      count<< *p <<endl;//报错 error
  }
  
  //2 堆区开辟一个数组
  void test01()
  {
      //创建10整形数据的数组，在堆区
     	int* arr = new int[10];//10代表数组有10个元素
      
      for(int i=0;i<10;i++)
      {
          arr[i] = i + 10；
      }
      
      for(int i=0;i<10;i++）
      {
  		cout<< arr[i]<<endl;
      }  
          
       //释放堆区数组
       //释放数组的时候，要加[]才可以
          
       delete[] arr;
  
          
  }
  
  int main(){
  	test 01()；
      test 02()；
  }
  ```

  

