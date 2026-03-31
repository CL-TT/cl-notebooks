# 深度优先(DFS)和广度优先(BFS)遍历

## 1. 深度优先遍历

1. 它的主要思想是从图(树是图的一种特例)中的一个未访问的顶点V开始，沿着一条路一直走到底，然后从这条路尽头的节点回退到上一个节点，再从另一条路开始走到底，不断重复此过程，直到所有的顶点都遍历完，这就是深度优先遍历。



2. 我们来看一下该树如何用深度遍历

![tree1](D:\CL武林秘籍\画图\tree1.png)

```js
1. 先找到一个未访问的顶点， 1，从这个顶点开始遍历

2. 沿着一条路一直走到底，从最左边开始， 依次遍历2， 5， 9
```

![tree2](D:\CL武林秘籍\画图\tree2.png)

```js
1. 到了9节点，因为已经是叶子节点，没有路可以走了，就回退到上一个节点，5节点除了9节点没有任何子节点，再回退到上一个节点2节点同样除了5节点没有任何子节点，再回退到上一个节点1节点，1节点有其余的子节点

2. 还是从最左边开始，依次是3， 6， 10
```

![tree3](D:\CL武林秘籍\画图\tree3.png)

```js
1. 10节点同样是叶子结点，回退到上一个节点，6节点除了10没有任何子节点，再回退到上一个节点，3节点除了6节点还有一个7节点

2. 所以遍历 7
```

![tree4](D:\CL武林秘籍\画图\tree4.png)

```js
1. 回退到1节点之后，然后就是遍历4， 8

2. 总的遍历顺序如下图  1,2,5,9,3,6,10,7,4,8

3. 不难发现这其实就是树的前序遍历， 其实不管是前序，中序，后序遍历都是深度优先遍历
```

![tree5](D:\CL武林秘籍\画图\tree5.png)

#### 2. 我用js来实现一下二叉树的深度优先遍历

![tree6](D:\CL武林秘籍\画图\tree6.png)

```js
function Node(value){
    this.value = value;
    this.left = null;
    this.right = null;
}

const A = new Node('A');
const B = new Node('B');
const C = new Node('C');
const D = new Node('D');
const E = new Node('E');
const F = new Node('F');

A.left = B;
A.right = C;
B.right = D;
C.left = E;
C.right = F;

深度优先遍历包括了前序，中序，后序

1. 前序遍历, 根左右

/**
* root: 传入一个根节点
**/
function DLR(root){
    if(root === null) return;
    
    console.log(root.value);
    
    DLR(root.left);
    DLR(root.right);
}

2. 中序遍历， 左根右

function LDR(root){
    if(root === null)return;
    
    LDR(root.left);
    console.log(root.value);
    LDR(root.right);
}

3. 后序遍历， 左右根

function LRD(root){
    if(root === null)return;
    
    LRD(root.left);
    LRD(root.right);
    console.log(root.value);
}

4. 给定一个节点，看是否存在于二叉树中

/**
* root: 传入一个根节点
* target: 目标节点
**/
function DFS(root, target){
    if(root === null || target === null) return false;
    
    if(root.value === target) return true;
    
    let left = DFS(root.left, target);
    let right = DFS(root.right, target);
    return left || right;
}

console.log(DFS(A, 'C'));    //true
console.log(DFS(A, 'N'));    //false
```



## 2. 广度优先遍历

1. 广度优先遍历，指的是从图的一个未遍历的节点出发，先遍历这个节点的相邻接点，再依次遍历每个相邻接点的相邻接点

![tree1](D:\CL武林秘籍\画图\tree1.png)

```js
1. 先从1出发，依次是2，3，4， 然后是5， 6， 7， 8， 再是9， 10
```



#### 2. 利用js完成二叉树的广度搜索

![tree6](D:\CL武林秘籍\画图\tree6.png)

```js
function Node(value){
    this.value = value;
    this.left = null;
    this.right = null;
}

const A = new Node('A');
const B = new Node('B');
const C = new Node('C');
const D = new Node('D');
const E = new Node('E');
const F = new Node('F');

A.left = B;
A.right = C;
B.right = D;
C.left = E;
C.right = F;

/**
* 广度优先不像深度那样，简单的传入一个根节点就可以的，而是要传入一个节点集合，一层一层的找
* rootList: 传入一个节点集合
* target: 传入一个目标节点
**/
function BFS(rootList, target){
    if(rootList === null || rootList.length === 0 || target === null) return false;
    
    let childList = [];
    
    for(let i = 0; i < rootList.length; i++){
        if(rootList[i] !== null && rootList[i].value === target)return true
        
        childList.push(rootList[i].left, rootList[i].right);
    }
    
    return BFS(childList, target);
}

console.log(BFS([A], 'c'));    //true
console.log(BFS([A], 'N'));    //false
```

