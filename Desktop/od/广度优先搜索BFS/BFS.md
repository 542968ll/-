###### 50、【200】二叉树层序遍历

![image-20230516015206318](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015206318.png)

![image-20230516015227609](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015227609.png)

![image-20230516015244204](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015244204.png)

![image-20230516015305494](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015305494.png)

![image-20230516015328073](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015328073.png)

![image-20230516015338993](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015338993.png)

![image-20230516015351722](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015351722.png)

![image-20230516015402524](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015402524.png)

```js

/* JavaScript Node ACM模式 控制台输入获取 */
const readline = require("readline");
 
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
 
rl.on("line", (line) => {
  const [post, mid] = line.split(" ");
  console.log(getResult(post, mid));
});
 
function getResult(post, mid) {
  // 广度优先搜索的执行队列，先加入左子树，再加入右子树
  const queue = [];
  // 层序遍历出来的元素存放在ans中
  const ans = [];
 
  devideLR(post, mid, queue, ans);
 
  while (queue.length) {
    const [post, mid] = queue.shift();
    devideLR(post, mid, queue, ans);
  }
 
  return ans.join("");
}
 
/**
 * 本方法用于从后序遍历、中序遍历序列中分离出：根，以及其左、右子树的后序、中序遍历序列
 * @param {*} post 后序遍历结果
 * @param {*} mid 中序遍历结果
 * @param {*} queue BFS的执行队列
 * @param {*} ans 题解
 */
function devideLR(post, mid, queue, ans) {
  // 后序遍历的最后一个元素就是根
  let rootEle = post.at(-1);
  // 将根加入题解
  ans.push(rootEle);
 
  // 在中序遍历中找到根的位置rootIdx，那么该位置左边就是左子树，右边就是右子树
  let rootIdx = mid.indexOf(rootEle);
 
  // 左子树长度，左子树是中序遍历的0~rootIdx-1范围，长度为rootIdx
  let leftLen = rootIdx;
 
  // 如果存在左子树，即左子树长度大于0
  if (leftLen > 0) {
    // 则从后序遍历中，截取出左子树的后序遍历
    let leftPost = post.slice(0, leftLen);
    // 从中序遍历中，截取出左子树的中序遍历
    let leftMid = mid.slice(0, rootIdx);
    // 将左子树的后、中遍历序列加入执行队列
    queue.push([leftPost, leftMid]);
  }
 
  // 如果存在右子树，即右子树长度大于0
  if (post.length - 1 - leftLen > 0) {
    // 则从后序遍历中，截取出右子树的后序遍历
    let rightPost = post.slice(leftLen, post.length - 1);
    // 从中序遍历中，截取出右子树的中序遍历
    let rightMid = mid.slice(rootIdx + 1);
    // 将右子树的后、中遍历序列加入执行队列
    queue.push([rightPost, rightMid]);
  }
}
```



###### 10、【200】污染水域

![image-20230516015525464](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015525464.png)

![image-20230516015536995](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015536995.png)

```js

/* JavaScript Node ACM模式 控制台输入获取 */
const readline = require("readline");
 
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
 
rl.on("line", (line) => {
  const arr = line.split(",").map(Number);
  const n = Math.sqrt(arr.length);
  let total = arr.length; // 未污染区域数量
 
  const matrix = new Array(n).fill(0).map(() => new Array(n));
  let queue = [];
 
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      matrix[i][j] = arr[n * i + j];
      if (matrix[i][j] === 1) {
        queue.push([i, j]);
        total--;
      }
    }
  }
 
  if (queue.length === 0 || queue.length === arr.length) {
    return console.log(-1);
  }
 
  const offset = [
    [-1, 0],
    [1, 0],
    [0, -1],
    [0, 1],
  ];
 
  while (queue.length && total) {
    const newQueue = [];
 
    for (const [i, j] of queue) {
      const num = matrix[i][j];
 
      for (let m = 0; m < offset.length; m++) {
        const [offsetX, offsetY] = offset[m];
 
        const newI = i + offsetX;
        const newJ = j + offsetY;
 
        if (
          newI >= 0 &&
          newI < n &&
          newJ >= 0 &&
          newJ < n &&
          matrix[newI][newJ] === 0
        ) {
          matrix[newI][newJ] = num + 1;
          newQueue.push([newI, newJ]);
          total--;
 
          if (total === 0) {
            console.log(num);
            return;
          }
        }
      }
    }
 
    queue = newQueue;
  }
});
```





###### 51、【200】矩阵扩散

###### ![image-20230516015657223](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015657223.png)

![image-20230516015714346](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015714346.png)

```js
/* JavaScript Node ACM模式 控制台输入获取 */
const readline = require("readline");
 
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
 
rl.on("line", (line) => {
  const [m, n, i, j, k, l] = line.split(",").map(Number);
  console.log(getResult(m, n, i, j, k, l));
});
 
/**
 * @param {*} m m×n的二维数组
 * @param {*} n m×n的二维数组
 * @param {*} i 扩散点位置为i,j
 * @param {*} j 扩散点位置为i,j
 * @param {*} k 扩散点位置为k,l
 * @param {*} l 扩散点位置为k,l
 */
function getResult(m, n, i, j, k, l) {
  const matrix = new Array(m).fill(0).map(() => new Array(n).fill(0));
  matrix[i][j] = 1;
  matrix[k][l] = 1;
 
  // count记录未被扩散的点的数量
  let count = m * n - 2;
 
  // 多源BFS实现队列
  let queue = [
    [i, j],
    [k, l],
  ];
 
  // 上下左右偏移量
  const offsets = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ];
 
  let day = 1;
  // 如果扩散点没有了，或者所有点已被扩散，则停止循环
  while (queue.length && count) {
    const newQueue = [];
 
    for (const [x, y] of queue) {
      // 我们假设初始扩散点的1代表第1秒被扩散到的，则下一波被扩散点的值就是1+1，即第2秒被扩散到的
      day = matrix[x][y] + 1;
 
      for (let offset of offsets) {
        const [offsetX, offsetY] = offset;
        const newX = x + offsetX;
        const newY = y + offsetY;
 
        if (
          newX >= 0 &&
          newX < m &&
          newY >= 0 &&
          newY < n &&
          matrix[newX][newY] === 0
        ) {
          // 将点被扩散的时间记录为该点的值
          matrix[newX][newY] = day;
          // 被扩散到的点将变为新的扩散源
          newQueue.push([newX, newY]);
          // 未被扩散点的数量--
          count--;
        }
      }
    }
 
    queue = newQueue;
  }
 
  return day - 1;
}
```



###### 59、【200】疫情扩散的时间

![image-20230516015843150](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015843150.png)

![image-20230516015856856](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015856856.png)

![image-20230516015915109](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015915109.png)

![image-20230516015942543](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516015942543.png)

![image-20230516020003493](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516020003493.png)

![image-20230516020019237](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516020019237.png)

```js

/* JavaScript Node ACM模式 控制台输入获取 */
const readline = require("readline");
 
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
 
rl.on("line", (line) => {
  const arr = line.split(",").map(Number);
  console.log(howLong(arr));
});
 
function howLong(arr) {
  // 题目说会输入n*n个值
  const n = Math.sqrt(arr.length);
 
  // 将一维输入转为二维矩阵
  const matrix = new Array(n).fill(0).map(() => new Array(n).fill(0));
 
  // 将矩阵中所有感染区域位置记录到queue中,这里选择queue先进先出的原因是保证当天的感染区域并发扩散
  let queue = [];
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      matrix[i][j] = arr[i * n + j];
      if (matrix[i][j] === 1) queue.push([i, j]);
    }
  }
 
  // 全是感染区，或全是健康区
  if (queue.length === 0 || queue.length === arr.length) {
    return -1;
  }
 
  // 健康区个数
  let healthy = arr.length - queue.length;
 
  // 上下左右位置偏移量
  const offset = [
    [-1, 0], // 上
    [1, 0], // 下
    [0, -1], // 左
    [0, 1], // 右
  ];
 
  // day用于统计感染全部花费的时间，理论上应该从1开始统计，但是这里从2开始统计，因此最终结果要减去1，为什么从2开始统计，是因为这里的day既要统计天数，也要用于标记感染区，由于1已经被标记为感染区，因此新的感染区只能从2开始标记
  let day;
  // 如果健康区个数为0，说明感染完了
  while (queue.length && healthy) {
    const newQueue = [];
    for (const [x, y] of queue) {
      day = matrix[x][y] + 1;
 
      for (let i = 0; i < offset.length; i++) {
        const [offsetX, offsetY] = offset[i];
        const newX = x + offsetX;
        const newY = y + offsetY;
 
        if (newX < 0 || newX >= n || newY < 0 || newY >= n) continue;
 
        if (matrix[newX][newY] === 0) {
          healthy--;
          matrix[newX][newY] = day;
          newQueue.push([newX, newY]);
        }
      }
    }
    queue = newQueue;
  }
 
  return day - 1;
}
```



###### 【200】计算网络信号、信号强度

![image-20230516020209573](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516020209573.png)

![image-20230516020237377](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516020237377.png)

![image-20230516020249314](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516020249314.png)

![image-20230516020306406](C:\Users\GG-BOND\Desktop\od\广度优先搜索BFS\image-20230516020306406.png)

```js

/* JavaScript Node ACM模式 控制台输入获取 */
const readline = require("readline");
 
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
 
const lines = [];
rl.on("line", (line) => {
  lines.push(line);
 
  if (lines.length === 3) {
    const [m, n] = lines[0].split(" ").map(Number);
    const arr = lines[1].split(" ").map(Number);
    const pos = lines[2].split(" ").map(Number);
    console.log(getResult(arr, m, n, pos));
    lines.length = 0;
  }
});
 
function getResult(arr, m, n, pos) {
  let queue = [];
 
  for (let i = 0; i < m; i++) {
    for (let j = 0; j < n; j++) {
      if (arr[i * n + j] > 0) {
        queue.push([i, j]);
        break;
      }
    }
  }
 
  // 上下左右偏移量
  const offset = [
    [-1, 0],
    [1, 0],
    [0, -1],
    [0, 1],
  ];
 
  while (queue.length) {
    const newQueue = []; // 此时不使用queue.shift操作，因为此步操每次都需要O(n)时间，无法应对大数量级情况
 
    for (const [i, j] of queue) {
      const x = arr[i * n + j] - 1;
 
      if (x === 0) break;
 
      for (let [offsetX, offsetY] of offset) {
        const newI = i + offsetX;
        const newJ = j + offsetY;
 
        if (
          newI >= 0 &&
          newI < m &&
          newJ >= 0 &&
          newJ < n &&
          arr[newI * n + newJ] === 0
        ) {
          arr[newI * n + newJ] = x;
          newQueue.push([newI, newJ]);
        }
      }
    }
 
    queue = newQueue;
  }
 
  const [tarI, tarJ] = pos;
  return arr[tarI * n + tarJ];
}
```

