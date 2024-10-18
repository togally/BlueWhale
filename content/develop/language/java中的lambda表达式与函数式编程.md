+++
title = 'java中的lambda表达式与函数式编程'
series = ["编程语言"]
tags = ['java']
date = 2024-09-14T23:09:00+08:00
draft = false
+++
## 定义

lambda表达式允许我们把函数当作一个方法的入参传入

是java中的一个重要特性。

## 结构

（parameters）-> expression;

（parameters）-> {statements};

## 函数式接口

只声明了一个抽象函数的接口就是函数式接口，我们可以使用@FunctionInterface来保证该接口是一个函数式接口。

我们使用lambda表达式实际上就是生成一个函数式接口的对象。

### java内置的函数式接口

![hlambda_1](java/lambda_1.png)

## 方法引用

方法引用可以理解为lambda表达式的另一种表现形式。当函数式接口所需要用的对应的方法已经有了实现时可以使用方法引用。

```PowerShell
Function<Integer, Integer> square = Math::square;
int result = square.apply(4); // 结果为 16
```

```PowerShell
Function<String, Integer> strLength = String::length;
int result = strLength.apply("Hello"); // 结果为 5
```

```PowerShell
Function<String, Person> createPerson = Person::new;
Person person = createPerson.apply("Alice");
```

