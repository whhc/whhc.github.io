---
layout: post
title:  "Binary Tree 1"
date:   2017-10-23 22:27:00 +0800
categories: articles
tags: BinaryTree, Algorithms
---

### 列表表示

```python
myTree = [ 'a', #root
    ['b', #left subtree
    ['d',[],[]],
    ['e',[],[]] ],
    ['c', # right subtree
    ['f',[], []],
    []]
]
```
可以用列表索引来访问子树，如根是`myTree[0]`,左子树是`myTree[1]`,右子树是`myTree[1]`:

```python
myTree = ['a',['b',['d',[],[]],['e',[],[]]],['c',['f',[],[]]]]
print(myTree)
print('left subtree =', myTree[1])
print('root =', myTree[0])
print('right subtree =', myTree[2])
```

#### 树结构数组 

```python
def BinaryTree(r):
    return [r,[],[]]
```
在`BinaryTree`中添加左子树，则需要在根列表第二个位置添加一个新的列表，并且如果此位置列表存在，则沿树向下添加：
```python
def insertLeft(root, newBranch):
    t = root.pop(1)
    if len(t) > 1:
        root.insert(1, [newBranch, t, []])
    else:
        root.insert(1,[newBranch, [], []])
    return root
```
以上代码，首先获得可能为空的与当前子节点对应的列表，然后添加新的左子树，添加旧的左子树作为新节点的左子节点。添加右节点类似:
```python
def insertRight(root, newBranch):
    t = root.pop(2)
    if len(t) > 1:
        root.insert(2, [newBranch, [], t])
    else:
        root.insert(2, [newBranch, [], t])
    return root
```

### 节点表示

```python
class BinaryTree:
    def __init__(self, rootObj):
        self.key = rootObj
        self.leftChild = None
        self.rightChild = None
    
    def insertLeft(self, newNode):
        if self.leftChild == None:
            self.leftChild = BinaryTree(newNode)
        else:
            t = BinaryTree(newNode)
            t.leftChild = self.leftChild
            self.leftChild = t
    
    def insertRight(self, newNode):
        if self.rightChild == None:
            self.rightChild = BinaryTree(newNode)
        else:
            t = BinaryTree(newNode)
            t.rightChild = self.rightChild
            self.rightChild = t
    
    def getRightChild(self):
        return self.rightChild

    def getLeftChild(self):
        return self.leftChild

    def setRootVal(self, obj):
        self.key = obj
    
    def getRootVal(self):
        return self.key
```

## 分析树


如`(7+3)*(5-2)`:     
- 如何从数学表达式中构建分析树
- 如何评估存储在分析树中的表达式
- 如何从分析树中恢复原始的数学表达式

规则如下：       

- 如果当前符号是`(`，添加一个新节点作为当前节点的子节点，病下降到左子节点
- 如果当前符号在列表`['+','-','*','/']`中，将当前节点的根值设置位由当前符号表示的运算符。添加一个新节点作为当前节点的右子节点，并下降到右子节点
- 如果当前符号是数字，将当前节点的根值设置为该数字并返回到父节点
- 如果当前符号是`)`，则转到当前节点的父节点。

[Github pythonds](https://github.com/bnmnetp/pythonds)

```python
from pythonds.basic.stack import Stack
from pythonds.trees.binaryTree import BinaryTree

def buildPasreTree(fpexp):
    fplist = fpexp.split()
    pStact = Stack()
    eTree = BinaryTree('')
    pStack.push(eTree)
    currentTree = eTree
    for i in fplist:
        if i == '(':
            currentTree.insertLeft('')
            pStack.push(currentTree)
            currentTree = currentTree.getLeftChild()
        elif i not in ['+','-','*','/',')']:
            currentTree.setRootVal(int(i))
            parent = pStack.pop()
            currentTree = parent
        elif i in ['+','-','*','/']:
            currentTree.setRootVal(i)
            currentTree.insertRight('')
            pStack.push(currentTree)
            currentTree = currentTree.getRightChild()
        elif i == ')':
            currentTree = pStack.pop()
        else:
            raise ValueError
        
    return eTree

    pt = buildParseTree("((10 + 5) * 3)")

    pt.postorder() # defined and explained in the next section
```
对树的验证：
```python
def evaluate(parseTree):
    opers = {'+':operator.add, '-':operator.sub, '*':operator.mul, '/':operator.truediv}

    leftC = parseTree.getLeftChild()
    rightC = parseTree.getRightChild()

    if leftC and rightC:
        fn = opers[parseTree.getRootVal()]
        return fn(evaluate(leftC),evaluate(rightC))
    else:
        return parseTree.getRootVal()
```

