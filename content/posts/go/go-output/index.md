---
title: "Golang：程序预期输出"
date: "2024-01-11"
description: ""
tags: ["Golang"]
categories: ["Golang"]
---

## 代码1：
```golang
func calc(index string, a, b int) int {
	ret := a + b
	fmt.Println(index, a, b, ret)
	return ret
}

func main() {
	a := 1
	b := 2
	defer calc("1", a, calc("10", a, b))
	a = 0
	defer calc("2", a, calc("20", a, b))
	b = 1
}
```

### 输出：
```shell
10 1 2 3
20 0 2 2  
2 0 2 2
1 1 3 4
```
### 解释：
defer 在定义的时候会计算好调用函数的参数，所以先顺序执行两个defer里面的calc，然后再倒序执行defer

## 代码2：
```golang
func main() {
	runtime.GOMAXPROCS(1)
	int_chan := make(chan int, 1)
	string_chan := make(chan string, 1)
	int_chan <- 1
	string_chan <- "hello"
	select {
	case value := <-int_chan:
		fmt.Println(value)
	case value := <-string_chan:
		panic(value)
	}
}
```

### 输出：
```shell
1 or panic
```

### 解释：
golang 在多个case 可读的时候会公平的选中一个执行

## 代码3：
```golang
func main() {
	s := make([]int, 5)
	s = append(s, 1, 2, 3)
	fmt.Println(s)
}
```

### 输出：
```shell
0 0 0 0 0 1 2 3
```

### 解释：
make 在初始化切片时指定了长度，数据初始化为默认值，追加数据时会从len(s) 位置开始填充数据

## 代码4:
```golang
type People interface {
	Speak(string) string
}

type Student struct{}

func (stu *Student) Speak(think string) (talk string) {
	if think == "bitch" {
		talk = "You are a good boy"
	} else {
		talk = "hi"
	}
	return
}

func main() {
	var peo People = Student{}
	think := "bitch"
	fmt.Println(peo.Speak(think))
}
```

### 输出：
编译失败

### 解释：
在 golang 语言中，Student 和 *Student 是两种类型，第一个是表示 Student 本身，第二个是指向 Student 的指针，
值类型 Student{} 未实现接口People的方法，不能定义为 People类型
