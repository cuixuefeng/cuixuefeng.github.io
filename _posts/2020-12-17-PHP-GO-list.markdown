---
layout:     post
title:      "PHP,GOlang 链表"
subtitle:   " 面试经常考。。"
date:       2020-12-21 22:01:00
author:     "cuizhazha"
header-img: ""
header-bg-css: "linear-gradient(to right, #24b94a, #38ef7d);"
catalog: true
tags:
    - PHP
---



### 1\. 什么是链表?

> 链表（Linked list）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针(Pointer)。由于不必须按顺序存储，链表在插入的时候可以达到O(1)的复杂度，比另一种线性表[顺序表](https://zh.wikipedia.org/wiki/%E9%A1%BA%E5%BA%8F%E8%A1%A8)快得多，但是查找一个节点或者访问特定编号的节点则需要O(n)的时间，而顺序表相应的时间复杂度分别是O(logn)和O(1)。


### 2\. 链表的组成部分

> 链表通常由一连串节点组成，每个节点包含任意的实例数据（信息域 ）和一或两个用来指向上一个/或下一个节点的位置的链接（链域）

### 3\. 数组和链

*   链表结构可以克服数组链表需要预先知道数据大小的缺点，链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理

### 4\. 数组与链表的优缺点

1.  存取方式上，数组可以顺序存取或者随机存取，而链表只能顺序存取；
2.  存储位置上，数组逻辑上相邻的元素在物理存储位置上也相邻，而链表不一定；　
3.  存储空间上，链表由于带有指针域，存储密度不如数组大；　
4.  按序号查找时，数组可以随机访问，时间复杂度为O(1)，而链表不支持随机访问，平均需要O(n)；　
5.  按值查找时，若数组无序，数组和链表时间复杂度均为O(n)，但是当数组有序时，可以采用折半查找将时间复杂度降为O(logn)；
6.  插入和删除时，数组平均需要移动n/2个元素，而链表只需修改指针即可；
7.  空间分配方面：
    
    > 数组在静态存储分配情形下，存储元素数量受限制，动态存储分配情形下，虽然存储空间可以扩充，但需要移动大量元素，导致操作效率降低，而且如果内存中没有更大块连续存储空间将导致分配失败；  
    > 　　链表存储的节点空间只在需要的时候申请分配，只要内存中有空间就可以分配，操作比较灵活高效；
    
8.  经典数据结构涵盖了多种抽象数据类型（ADT），其中包括栈、队列、有序列表、排序表、哈希表及分散表、树、优先队列、集合和图等。对于每种情况，都可以选用数组或某一种链表数据结构来实现其抽象数据类型（ADT）。由于数组和链表几乎是建立所有ADT的基础，所以称数组与链表为基本数据结构

### 5\. 链表类型

*   单向链表
*   双向链表
*   循环链表

### 6\. 单向链表

> 链表中最简单的一种是单向链表，它包含两个域，一个信息域和一个指针域。这个链接指向列表中的下一个节点，而最后一个节点则指向一个空值。

`一个单向链表的节点被分成两个部分。第一个部分保存或者显示关于节点的信息，第二个部分存储下一个节点的地址。单向链表只可向一个方向遍历。`




### 7\. 代码（PHP）实现单向链表

~~~php
<?php

/**
 * Class Node
 */
class Node
{
    /**
     *
     * @var
     */
    private $Data;//数据集
    /**
     *
     * @var
     */
    private $Next;//下一个节点

    /**
     * Node constructor.
     * @param $next
     * @param $data
     */
    public function __construct($data, $next)
    {
        $this->setData($data);
        $this->setNext($next);

    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param $data
     */
    public function setData($data)
    {
        $this->Data = $data;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @return mixed
     */
    public function getData()
    {
        return $this->Data;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param $next
     */
    public function setNext($next)
    {
        $this->Next = $next;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @return mixed
     */
    public function getNext()
    {
        return $this->Next;
    }

}

/**
 * Class LinkList
 */
class LinkList
{
    /**
     *
     * @var
     */
    private $header;//链表头部信息
    /**
     *
     * @var
     */
    private $len;//链表长度


    /**
     * LinkList constructor.
     */
    public function __construct()
    {
        $this->setHeader(new Node(null, null));
        $this->len = 0;
    }


    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param Node $node
     */
    public function setHeader(Node $node)
    {
        $this->header = $node;
    }

    /**
     *  Functional description : 获取当前节点
     *  Programmer : cuizhazha
     * @return Node
     */
    public function getHeader()
    : Node
    {
        return $this->header;
    }


    /**
     *  Functional description : 获取链表长度
     *  Programmer : cuizhazha
     * @return mixed
     */
    public function getLen()
    : int
    {
        return $this->len;
    }


    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param $data
     */
    public function add(string $data)
    {
        $node = $this->getHeader();
        //查找没有子节点的节点
        while ($node->getNext() != null) {
            $node = $node->getNext();
        }
        //找到后设置其下级节点
        $node->setNext(new Node($data, null));

        //链表长度递增
        $this->len++;
    }


    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param Node $currentNode
     * @param Node $prevNode
     *
     * 示例：A-B-C ==> A-C
     * @return bool
     */
    private function delLink(Node $currentNode, Node $prevNode)
    {
        $prevNode->setNext($currentNode->getNext());
        $this->len--;
        return true;
    }

    /**
     *  Functional description : 根据节点的数据进行删除
     *  Programmer : cuizhazha
     * @param $data
     */
    public function delByData($data)
    {
        $node     = $this->getHeader();
        $prevNode = $node;
        //查找没有数据一致的节点
        while ($node->getData() != $data) {
            $prevNode = $node;
            $node     = $node->getNext();
        }
        //删除(把要删的节点A下的子节点位置移到A节点，从而删除)
        $this->delLink($node, $prevNode);
    }


    /**
     *  Functional description : 删除第一个节点0~N
     *  Programmer : cuizhazha
     * @param int $index
     * @return bool
     */
    public function delByIndex(int $index)
    {
        $node = $prevNode = $this->getHeader();
        $i    = 0;
        if ($i > $this->getLen() -1 ){
            //节点超
            return false;
        }
        while ($i != $index) {
            if ($node->getNext() == null) {
                return false;
            }
            $prevNode = $node;
            $node     = $node->getNext();
            $i++;
        }
        return $this->delLink($node, $prevNode);
    }

    /**
     *  Functional description : 获取链表上所有数据
     *  Programmer : cuizhazha
     * @return array
     */
    public function getLinkData()
    {
        $node = $this->getHeader();
        $list = [];

        //根节点处理
        if ($node->getData() != null) {
            $list[] = $node->getData();
        }

        //从下一个节点开始获取数据
        $node = $node->getNext();
        if ($node->getData() == null) {
            return $list;
        }
        while ($node->getData() != null && $node->getNext() != null) {
            $list[] = $node->getData();
            $node   = $node->getNext();
        }
        return $list;
    }


    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * 添加D到1示例：A-B-C ==> A-D-B-C
     * @param int $index
     * @param string $data
     * @return bool
     */
    public function addByIndex(int $index,string $data){
        $node  = $this->getHeader();
        $i=0;
        while ($i != $index){
            if ($node->getNext() == null) {
                return false;
            }
            $node = $node->getNext();
            $i++;

        }
        //进行添加
        $node->setNext(new Node($data,$node->getNext()));
        $this->len++;
        return true;
    }

}
~~~
### 8\. 什么是双向链表?

> 双向链表(双链表)又叫双面链表,双向链表中不仅有指向后一个节点的指针，还有指向前一个节点的指针。这样可以从任何一个节点访问前一个节点，当然也可以访问后一个节点，以至整个链表。一般是在需要大批量的另外储存数据在链表中的位置的时候用。双向链表也可以配合下面的其他链表的扩展使用。

`由于另外储存了指向链表内容的指针，并且可能会修改相邻的节点，有的时候第一个节点可能会被删除或者在之前添加一个新的节点。这时候就要修改指向首个节点的指针。有一种方便的可以消除这种特殊情况的方法是在最后一个节点之后、第一个节点之前储存一个永远不会被删除或者移动的虚拟节点，形成一个下面说的循环链表。这个虚拟节点之后的节点就是真正的第一个节点。这种情况通常可以用这个虚拟节点直接表示这个链表，对于把链表单独的存在数组里的情况，也可以直接用这个数组表示链表并用第0个或者第-1个（如果编译器支持）节点固定的表示这个虚拟节点。`


### 9\. 什么是循环链表?

> 循环链表指的是首节点和末节点被连接在一起的链表，这种方式在单向和双向链表中皆可实现。循环链表的无边界使得在这样的链表上设计算法会比普通链表更加容易。对于新加入的节点应该是在第一个节点之前还是最后一个节点之后可以根据实际要求灵活处理。  
> 


### 10\. 单向链表与双向链表优缺点

#### 10.1 单向链表

**优点：**

*   单向链表增加删除节点简单。遍历时候不会死循环。（双向也不会死循环，循环链表忘了进行控制的话很容易进入死循环）

**缺点：**

*   只能从头到尾遍历。只能找到后继，无法找到前驱，也就是只能前进。

#### 10.2 双向链表

**优点：**

*   可以找到前驱和后继，可进可退。

**缺点：**

*   增加删除节点复杂（其实就复杂一点点）

### 11\. 代码（PHP）实现双向链表

~~~php
<?php
/**
 * Class Node
 */
class Node
{
    /**
     *
     * @var
     */
    private $Data;//数据集
    /**
     *
     * @var
     */
    private $Prev;//上一个节点
    /**
     *
     * @var
     */
    private $Next;//下一个节点

    /**
     * Node constructor.
     * @param $next
     * @param $data
     * @param $prev
     */
    public function __construct($data, $next,$prev)
    {
        $this->setData($data);
        $this->setNext($next);
        $this->setPrev($prev);
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param $data
     */
    public function setData($data)
    {
        $this->Data = $data;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @return mixed
     */
    public function getData()
    {
        return $this->Data;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param $next
     */
    public function setNext($next)
    {
        $this->Next = $next;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @return mixed
     */
    public function getNext()
    {
        return $this->Next;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param $prev
     */
    public function setPrev($prev)
    {
        $this->Prev = $prev;
    }

    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @return mixed
     */
    public function getPrev()
    {
        return $this->Prev;
    }

}

/**
 * Class LinkList
 */
class LinkList
{
    /**
     *
     * @var
     */
    private $header;

    /**
     *
     * @var
     */
    private $len;//链表长度


    /**
     * LinkList constructor.
     */
    public function __construct()
    {
        //初始化根节点
        $this->setHeader(new Node(null, null,null));
        $this->len = 0;
    }


    /**
     *  Functional description :
     *  Programmer : cuizhazha
     * @param Node $node
     */
    public function setHeader(Node $node)
    {
        $this->header = $node;
    }

    /**
     *  Functional description : 获取头部节点
     *  Programmer : cuizhazha
     * @return Node
     */
    public function getHeader()
    : Node
    {
        return $this->header;
    }


    /**
     *  Functional description : 获取链表长度
     *  Programmer : cuizhazha
     * @return mixed
     */
    public function getLen()
    : int
    {
        return $this->len;
    }


    /**
     *  Functional description : 末尾追加
     *  Programmer : cui
     * @param $data
     */
    public function append($data)
    {
        $node = $this->getHeader();
        //查找没有子节点的节点
        while ($node->getNext() != null) {
            $node = $node->getNext();
        }
        //找到后设置其下级节点
        $node->setNext(new Node($data, null,$node));
        //链表长度递增
        $this->len++;
    }


    /**
     *  Functional description : 头部添加
     *  Programmer : cuizha zha
     * @param $data
     */
    public function addFirst($data){
        $rootNode = $this->getHeader();
        //查找没有子节点的节点(根节点)
        while ($rootNode->getPrev() != null) {
            $rootNode = $rootNode->getPrev();
        }
        //Root-A-B-C Root-D-A-B-C
        $oldFirst = $rootNode->getNext();
        $newFirst = new Node($data,$oldFirst,$rootNode);
        $oldFirst->setPrev($newFirst);
        $rootNode->setNext($newFirst);
        $this->len++;
    }

    /**
     *  Functional description : 根据指定位置添加
     *  Programmer : cuizhazha
     * @param int $index
     * @param $data
     */
    public function insertByIndex(int $index,$data){
        $key = 1;
        //尾部添加
        if ($this->getLen() < $index){
            $this->append($data);
            return;
        }
        //头部添加
        if ($index == 1){
            $this->addFirst($data);
            return;
        }

        $node = $this->getHeader();
        while ($key < $index){
            $node = $node->getNext();
            $key++;
        }
        //中间添加 如：添加D到A和B之间 A-B-C  ---> A-D-B-C
        $node->setNext(new Node($data,$node->getNext(),$node));
        $this->len++;
    }


    /**
     *  Functional description : 删除第一个节点
     *  Programmer : cuizhazha
     */
    public function delFirst(){
        $rootNode = $this->getHeader();
        while ($rootNode->getPrev() !=null){
            $rootNode = $rootNode->getPrev();
        }
        //删除：ROOT-A-B-C  -->  ROOT-B-C
        $firstNode = $rootNode->getNext();
        $secondNode = $firstNode->getNext();
        $rootNode->setNext($secondNode);
        $secondNode->setPrev($rootNode);
        $this->len--;
    }

    /**
     *  Functional description : 删除最后一个节点
     *  Programmer : cuizhazha
     */
    public function delLast(){
        $node = $this->getHeader();
        while ($node->getNext() !=null){
            $node = $node->getNext();
        }
        //删除：ROOT-A-B-C  -->  ROOT-A-B
        $newLastNode = $node->getPrev();
        $newLastNode->setNext(null);
        $this->len--;
    }


    /**
     *  Functional description : 反转双向链表
     *  Programmer : cuizhazha
     * @return void
     */
    public function reverse(){
        if ($this->getHeader() != null){
            return ;
        }

        //除去根节点后的节点
        $currentNode = $this->getHeader()->getNext();
        //后一个节点
        $next=null;
        $prev=null;//根节点,当前为null
        //rootNode->0->1->2->3 -->  3->2->1->0
        //查找最后一个
        while ($currentNode != null){
            //用一个变量暂时存储后一节点，因为一旦前面反转，就断链了
            $next = $currentNode->getNext();
            //将前一节点作为当前节点的后一节点，是为反转
            $currentNode->setNext($prev);
            $currentNode->setPrev($next);

            #指针后移
            $prev = $currentNode;
            $currentNode = $next;

        }
        return $prev;
    }

}
~~~
# **2\. Golang中自定义实现链表**

![](https://static.oschina.net/uploads/space/2018/0516/095628_d7gC_2271515.png)

## 1.初始化Init

    双向链表的初始化，可以理解成准备买一个车队准备运煤。第一步，得获得国家有关部门的批准，有了批准就可以买车厢运煤了。但是，批准下来的时候，车队啥都没有，没有车头、车尾，连一节车厢也没有。Go语言代码实现：

~~~go
package main

//节点数据结构
type DNode struct {
	data interface{}
	prev *DNode
	next *DNode
}

// 链表数据结构
type DList struct {
	size uint64
	head *DNode
	tail *DNode
}

// 链表的初始化
func InitList() (list *DList) {
	list = *(DList)
	list.size = 0
	list.head = nil
	list.tail = nil
	return
}

// 新增数据
func (dlist *DList) Append(data interface{}) {
	// 创建一个节点
	newNode := &DNode{data: data}

	if (*dlist).GetSize() == 0 { //只有一个节点
		(*dlist).head = newNode
		(*dlist).tail = newNode // 头尾节点都是自己
		(*newNode).prev = nil   // 但是头尾的指向是nil
		(*newNode).next = nil
	} else { // 接到尾部
		// 新节点的指向修改
		(*newNode).prev = (*dlist).tail
		(*newNode).next = nil

		// 之前尾节点的指向修改
		(*(*dlist).tail).next = newNode // 将之前的尾节点的next指向新增节点

		// 更新链表的尾节点
		(*dlist).tail = newNode
	}

	// 更新链表的节点计数
	(*dlist).size++
}

// 在节点后面插入数据InsertNext
//param
//	- ele 所要插入的位置
//	- data 所要插入的节点数据
//
func (dlist *DList) InsertNext(ele *DNode, data interface{}) bool {
	if ele == nil {
		return false
	}

	if dlist.isTail(ele) { //正好在尾部
		dlist.Append(data)
	} else { // 在中间插入
		//构造新节点
		newNode := new(DNode)
		(*newNode).data = data
		(*newNode).prev = ele         // 上一个节点就是ele
		(*newNode).next = (*ele).next // 下一个节点就是ele原来的下一个节点

		// ele的下一个指向新节点
		(*ele).next = newNode

		// ele之前下节点的prev重新指向新节点
		*((*newNode).next).prev = newNode

		// 更新链表计数
		(*dlist).size++
	}
}

//*====
节点之前插入接口都是类似的方式：
	1. 首先根据数据创建新节点， 并设置指向
	2. 更新位置节点的指向数据
	3. 更新链表head , tail ,size数据

删除节点：
	1. 首先获取要删除节点指向数据
	(验证头尾)
	2. 更新要删除节点的prev节点的next为要删除节点的next节点（ 有点乱啊！！）
	3. 更新链表数据

	记得return要删除的节点数据（否则数据丢失）

查找节点：
	type MatchFun func (data1 interface{}, data2 interface{}) int
	func (dList *DList) Search(data Object, yourMatch MatchFun) *DNode

*=====*//

// 获取链表长度GetSize
func (dList *DList) GetSize() uint64 {
	return (*dList).size
}

//获取头部节点GetHead

func (dList *DList) GetHead() *DNode {
	return (*dList).head
}

//获取尾部节点GetTail

func (dList *DList) GetTail() *DNode {
	return (*dList).tail
}

~~~

通过自己实现链表，来更深入了解链表的结构后， 我们使用go的 container/list 库实现。

# **3.Go库container/list 实现链表操作**

关于库的成员函数 看一看文档很详细，也很简单。

代码：

~~~go

func main() {
	link := list.New()

	// 循环插入到头部
	for i := 0; i <= 10; i++ {
		link.PushBack(i)
	}

	// 遍历链表
	for p := link.Front(); p != link.Back(); p = p.Next() {
		fmt.Println("Number", p.Value)
	}

}

~~~

# **4. slice和list的性能比较**

1. 创建、 添加元素的比较

~~~go
package main

import (
	"container/list"
	"fmt"
	"time"
)
func main(){
  T1()
} 

func T1() {
	t := time.Now()
	//1亿创建添加测试
	// slice 创建

	slice := make([]int, 10)
	for i := 0; i < 1*100000*1000; i++ {
		slice = append(slice, i)
	}
	fmt.Println("slice 创建成功：", time.Now().Sub(t).String())

	// list创建添加
	t = time.Now()
	l := list.New()
	for i := 0; i < 1*100000*1000; i++ {
		l.PushBack(i)
	}
	fmt.Println("list创建成功：", time.Now().Sub(t).String())
}
func T2() {
	sli := make([]int, 10)
	for i := 0; i < 1*100000*1000; i++ {
		sli = append(sli, 1)
	}

	l := list.New()
	for i := 0; i < 1*100000*1000; i++ {
		l.PushBack(1)
	}
	// 比较遍历
	t := time.Now()
	for _, _ = range sli {
		//fmt.Printf("values[%d]=%d\n", i, item)
	}
	fmt.Println("遍历slice的速度:" + time.Now().Sub(t).String())
	t = time.Now()
	for e := l.Front(); e != nil; e = e.Next() {
		//fmt.Println(e.Value)
	}
	fmt.Println("遍历list的速度:" + time.Now().Sub(t).String())
}
func T3() {
	sli := make([]int, 10)
	for i := 0; i < 1*100000*1000; i++ {
		sli = append(sli, 1)
	}

	l := list.New()
	for i := 0; i < 1*100000*1000; i++ {
		l.PushBack(1)
	}
	//比较插入
	t := time.Now()
	slif := sli[:100000*500]
	slib := sli[100000*500:]
	slif = append(slif, 10)
	slif = append(slif, slib...)
	fmt.Println("slice 的插入速度" + time.Now().Sub(t).String())

	var em *list.Element
	len := l.Len()
	var i int
	for e := l.Front(); e != nil; e = e.Next() {
		i++
		if i == len/2 {
			em = e
			break
		}
	}
	//忽略掉找中间元素的速度。
	t = time.Now()
	ef := l.PushBack(2)
	l.MoveBefore(ef, em)
	fmt.Println("list: " + time.Now().Sub(t).String())
}

~~~

简单的测试下，如果频繁的插入和删除建议用list,频繁的遍历查询选slice。

**由于container/list不是并发安全的，所以需要自己手动添加一层并发的包装。**
# **5. 使用 Struct 定义单链表**
### 

利用 Struct 可以包容多种数据类型的特性，使用它作为链表的结点是最合适不过了。一个结构体内可以包含若干成员，这些成员可以是基本类型、自定义类型、数组类型，也可以是指针类型。这里可以使用指针类型成员来存放下一个结点的地址。  
  
【示例 1】使用 Struct 定义一个单向链表。

~~~
type Node struct {  

   Data  int  

  Next  *node}
~~~

其中成员 Data 用来存放结点中的有用数据，Next 是指针类型的成员，它指向 Node struct 类型数据，也就是下一个结点的数据类型。  
  
【示例 2】为链表赋值，并遍历链表中的每个结点。

~~~go
package main

import "fmt"

type Node struct { 

   data int 

   next *Node

 }

func Shownode(p *Node) { //遍历

    for p != nil {

        fmt.Println(*p)

        p = p.next //移动指针

    }}

func main() {

    var head = new(Node)

    head.data = 1

    var node1 = new(Node)

    node1.data = 2

    head.next = node1

    var node2 = new(Node)

    node2.data = 3

    node1.next = node2

    Shownode(head)}
~~~

运行结果如下：
~~~
{1 0xc00004c1e0}  
{2 0xc00004c1f0}  
{3 <nil>}
~~~
### 插入结点

单链表的结点插入方法一般使用头插法或者尾插法。  

#### 1 头插法

每次插入在链表的头部插入结点，代码如下所示：  

~~~go
package main

import "fmt"

type Node struct {

    data  int

    next  *Node

}

func Shownode(p *Node){   //遍历

    for p != nil{

        fmt.Println(*p)

        p=p.next  //移动指针    

   }

}

func main() {

    var head = new(Node)

    head.data = 0

    var tail *Node

    tail = head   //tail用于记录头结点的地址，刚开始tail的的指针指向头结点

    for i :=1 ;i<10;i++{

        var node = Node{data:i}

        node.next = tail   //将新插入的node的next指向头结点

        tail = &node      //重新赋值头结点

    }

    Shownode(tail) //遍历结果

}
~~~

运行结果如下:
~~~
{9 0xc000036270}  
{8 0xc000036260}  
{7 0xc000036250}  
{6 0xc000036240}  
{5 0xc000036230}  
{4 0xc000036220}  
{3 0xc000036210}  
{2 0xc000036200}  
{1 0xc0000361f0}  
{0 <nil>}
~~~
#### 2) 尾插法

每次插入结点在尾部，这也是我们较为习惯的方法。  

~~~go
package main
import "fmt"
type Node struct {   
 data  int    next  *Node

}

func Shownode(p *Node){ 

  //遍历   

 for p != nil{  

  fmt.Println(*p)        
p=p.next  //移动指针   

 }}

func main() {   

 var head = new(Node)  

  head.data = 0  

  var tail *Node  

  tail = head  

 //tail用于记录最末尾的结点的地址，刚开始tail的的指针指向头结点   

 for i :=1 ;i<10;i++{   

     var node = Node{data:i} 

       (*tail).next = &node        tail = &node    }    

Shownode(head) 

//遍历结果}
~~~

运行结果如下：

{0 0xc0000361f0}  
{1 0xc000036200}  
{2 0xc000036210}  
{3 0xc000036220}  
{4 0xc000036230}  
{5 0xc000036240}  
{6 0xc000036250}  
{7 0xc000036260}  
{8 0xc000036270}  
{9< nil>}

在进行数组的插入、删除操作时，为了保持内存数据的连续性，需要做大量的数据搬移，所以速度较慢。而在链表中插入或者删除一个数据，我们并不需要为了保持内存的连续性而搬移结点，因为链表的存储空间本身就不是连续的。所以，在链表中插入和删除一个数据是非常快速的。  
  
但是，有利就有弊。链表要想随机访问第 k 个元素，就没有数组那么高效了。因为链表中的数据并非连续存储的，所以无法像数组那样，根据首地址和下标，通过寻址公式就能直接计算出对应的内存地址，而是需要根据指针一个结点一个结点地依次遍历，直到找到相应的结点。

# **2\. 跳表**
对了还有跳表 可以参考下 [跳表](https://lotabout.me/2018/skip-list/)
