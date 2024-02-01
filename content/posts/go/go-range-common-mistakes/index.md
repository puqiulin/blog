---
title: "Golang：for range的“坑”"
date: "2024-01-15"
description: ""
categories: ["Golang"]
---
# 前言
某些情况下使用for range时很可能会被"坑"：在循环中使用引用、在循环中使用goroutines

## 在循环中使用引用

### 例子代码：
```golang
func main() {
	arr1 := []int{1, 2, 3}
	arr2 := make([]*int, len(arr1))

	for i, v := range arr1 {
		arr2[i] = &v
	}

	for _, v := range arr2 {
		fmt.Println(*v)
	}
}
```
1. 创建一个arr1，[]int，初始化 1,2,3 作为切片的值
2. 创建一个arr2，[]*int，初始化长度为arr1的长度
3. 通过for range遍历arr1，然后获取每一个元素的指针，赋值到对应arr2中
4. 逐行打印arr2中每个元素的值

### 预期输出：
```shell
1
2
3
```

### 实际结果：
```shell
3
3
3
```

### 原因
因为for range在遍历值类型时，其中的v变量是一个值的拷贝，变量会被复用，每次循环会将集合中的值复制给这个变量，当使用&获取指针时，
实际上是获取到v这个临时变量的指针， 而v变量在for range中只会创建一次，之后循环中会被一直重复使用，所以在arr2赋值的时候其实都是v变量的指针，
而&v最终会指向arr1最后一个元素的值拷贝。

### 解决方案

##### 1. 传递原始指针
```golang
func main() {
    arr1 := []int{1, 2, 3}
    arr2 := make([]*int, len(arr1))
    
    for i,_ := range arr1 {
        arr2[i] = &arr1[i]
    }
    
    for _, v := range arr2 {
        fmt.Println(*v)
    }
}
```

##### 2. 使用临时变量
```golang
func main() {
    arr1 := []int{1, 2, 3}
    arr2 := make([]*int, len(arr1))
    
    for i, v := range arr1 {
        t := v
        arr2[i] = &t
    }
    
    for _, v := range arr2 {
        fmt.Println(*v)
    }
}
```

##### 3. 使用闭包
```golang
func main() {
    arr1 := []int{1, 2, 3}
    arr2 := make([]*int, len(arr1))

    for i, v := range arr1 {
        func(v int){
            arr2[i] = &v
        }(v)
    }

    for _, v := range arr2 {
        fmt.Println(*v)
    }
}
```

## 在循环中使用goroutines

### 例子代码
```golang
func main() {
    wg := &sync.WaitGroup{}
    values := []int{1, 2, 3}
    
    for _, val := range values {
        wg.Add(1)
        go func(wg *sync.WaitGroup) {
            fmt.Print(val)
            wg.Done()
        }(wg)
    }
    
    wg.Wait()
}
```

### 预期输出：
```shell
123 or 132 or 213 or 231 or 321 or 312
```

### 实际结果：
```shell
333
```

### 原因
和上面在循环中使用引用一样，因为 goroutine 可能不会开始执行，直到循环之后，for range在遍历值类型时，其中的v变量是一个值的拷贝，变量会被复用，
每次循环会将集合中的值复制给这个变量，而不是顺序的每个值，而闭包只会绑定一个变量，即最后一个变量

### 解决方案

#### 通过将 v 作为参数添加到闭包中
```golang
func main() {
    wg := &sync.WaitGroup{}
    values := []int{1, 2, 3}
    
    for _, v := range values {
        wg.Add(1)
        go func(wg *sync.WaitGroup,v int) {
            fmt.Print(v)
            wg.Done()
        }(wg,v)
    }
    
    wg.Wait()
}
```

#### 或者
```golang
func main() {
    wg := &sync.WaitGroup{}
    values := []int{1, 2, 3}
    
    for i,_ := range values {
        v := values[i]
        wg.Add(1)
        go func(wg *sync.WaitGroup) {
            fmt.Print(v)
            wg.Done()
        }(wg)
    }
    
    wg.Wait()
}
```

## 注意

如果不将此闭包作为 goroutine 执行，代码将按预期运行。以下示例打印 1 到 10 之间的整数
```golang
func main() {
    for i := 1; i <= 10; i++ {
        func () {
            fmt.Print(i)
        }()
    }
}
```
输出
```shell
12345678910
```

## 官方链接
[using-reference-to-loop-iterator-variable](https://github.com/golang/go/wiki/CommonMistakes#using-reference-to-loop-iterator-variable)

