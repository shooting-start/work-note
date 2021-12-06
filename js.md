## script标签

- 位置

  过去放在`head`中，主要目的是把外部的 CSS 和 JavaScript 文件都集中放到一起。把所有 JavaScript 文件都放在里，也就意味着必须把所有 JavaScript 代码都下载、解析和解释完成后，才能开始渲 染页面（页面在浏览器解析到的起始标签时开始渲染）。对于需要很多 JavaScript 的页面，这会 导致页面渲染的明显延迟，在此期间浏览器窗口完全空白。

  现代 Web 应用程序通常 将所有 JavaScript 引用放在元素中的页面内容后面(`<body>底部`)，页面会在处理 JavaScript 代码之前渲染页面，浏览器显示空白页面的时间短了。

- 属性

  - defer

    立即下载，但延迟执行。脚本会被延迟到整个页面都解析完毕后再运行

  - async

    立即下载，立即执行。标记为 async 的脚本并不保证能按照它们出现的次序执行

  两者script标签异步加载，两者也都只适用于外部脚本。

## 排序算法

```
bubbleSort(arr,type) {
            if (type) {//降序
              for(let i = 0; i<arr.length-1;i++) {
                for(let j = 0;j<arr.length-i-1;j++) {
                  if(arr[j]<arr[j+1]) {
                    let tem = arr[j+1];
                    arr[j+1] = arr[j];
                    arr[j] = tem;
                  }
                }
              }
            } else {//升序
              for(let i = 0; i<arr.length-1;i++) {
                for(let j = 0;j<arr.length-i-1;j++) {
                  if(arr[j]>arr[j+1]) {
                    let tem = arr[j+1];
                    arr[j+1] = arr[j];
                    arr[j] = tem;
                  }
                }
              }
            }
            return arr;
          },
          
          selectionSort(arr,type) {
            if (type) {//降序
               for(let i = 0; i<arr.length-1;i++) {
                let maxIndex = i;
                for(let j = i+1; j<arr.length;j++) {
                  if (arr[maxIndex] < arr[j]) {
                    maxIndex = j;
                  }
                }
                let tem = arr[i];
                arr[i] = arr[maxIndex];
                arr[maxIndex] = tem;
              }

            } else {//升序
              for(let i = 0; i<arr.length-1;i++) {
                let minIndex = i;
                for(let j = i+1; j<arr.length;j++) {
                  if (arr[minIndex] > arr[j]) {
                    minIndex = j;
                  }
                }
                let tem = arr[i];
                arr[i] = arr[minIndex];
                arr[minIndex] = tem;
              }

            }
            return arr;

          },

          insertSort(arr,type) {
            let len = arr.length;
            if (type) {//降序
              for(let i = 1;i<len;i++) {
                let prev = i-1;
                let current = arr[i];
                while(prev>=0 && arr[prev]<current) {
                  arr[prev+1] = arr[prev];
                  prev -= 1;
                }
                arr[prev+1] = current;

              }
            } else {//升序
              for(let i = 1;i<len;i++) {
                let prev = i-1;
                let current = arr[i];
                while(prev>=0 && arr[prev]>current) {
                  arr[prev+1] = arr[prev];
                  prev -= 1;
                }
                arr[prev+1] = current;

              }
            }
            return arr;
          },

          shellSort(arr,type) {
            let len = arr.length;
            if (type) {
              for(let gap = Math.floor(len/2);gap>0;gap=Math.floor(gap/2) ) {
                
                for (let i = gap; i < len; i++) {
                  let prev = i-gap;
                  let current = arr[i];
                  while(prev>=0 && arr[prev] < current) {
                    arr[prev +gap] = arr[prev];
                    prev -= gap;
                  }
                  arr[prev+gap] = current;
                }
              }
            } else {
              for(let gap = Math.floor(len/2);gap>0;gap=Math.floor(gap/2) ) {
                
                for (let i = gap; i < len; i++) {
                  let prev = i-gap;
                  let current = arr[i];
                  while(prev>=0 && arr[prev] > current) {
                    arr[prev +gap] = arr[prev];
                    prev -= gap;
                  }
                  arr[prev+gap] = current;
                }
              }
            }
            return arr;
          },

          // 升序
          quickSort(arr,left,right) {
            // if(arr.length <=1 ) {
            //   return arr;
            // }
            // let pivot = arr[Math.floor(arr.length/2)];
            // let left = [];
            // let right = [];

            // for(let i = 0; i<arr.length;i++) {
            //   if(arr[i]<pivot) {
            //     left.push(arr[i]);
            //   } else if (arr[i]>pivot) {
            //     right.push(arr[i]);
            //   }
            // }
            // return this.quickSort(left).concat(pivot).concat(this.quickSort(right));

            if(left >= right) {
                return;
            }

            let pivot = arr[left];
            let i = left;
            let j = right;
            // 注意如果将arr[j]和基准交换一定要先从右往左找，先从左往右找到的i一定是大于基准的，但是j不一定是小于基准的
            //  [12,3,6,12,4,456,123,90] 先从左往右查找的话：i = 5 ,由于j = 5 ,arr[j]是大于基准12的 
            // 先从右往左查找的话：j = 4,arr[j]一定是小于基准的，
            while(i<j) {
              while (i<j && arr[j] >= pivot) {
                j--;
              }
              while (i<j && arr[i] <= pivot) {
                i++;
              }
              [arr[i],arr[j]] = [arr[j],arr[i]]
            }

            arr[left] = arr[j];
            arr[j] = pivot;
            
            this.quickSort(arr,left,j-1)
            this.quickSort(arr,j+1,right)

            // 反之要先从左往右找
            // pivot = arr[right];
            // while(i<j) {
            //   while (i<j && arr[i] <= pivot) {
            //     i++;
            //   }
            //   while (i<j && arr[j] >= pivot) {
            //     j--;
            //   }
            //   [arr[i],arr[j]] = [arr[j],arr[i]]
            // }

            // arr[right] = arr[i];
            // arr[i] = pivot;
            
            // this.quickSort(arr,left,i-1)
            // this.quickSort(arr,i+1,right)
            return arr;

          },
          // 降序
          quickSort_descending(arr,left,right) {
            if(left >= right) {
                return;
            }
            let pivot = arr[left];
            let i = left;
            let j = right;

            
            while(i<j) {
              while (i<j && arr[j] <= pivot) {
                j--;
              }
              while (i<j && arr[i] >= pivot) {
                i++;
              }
              [arr[i],arr[j]] = [arr[j],arr[i]]
            }
            arr[left] = arr[j];
            arr[j] = pivot;
            this.quickSort_descending(arr,left,j-1)
            this.quickSort_descending(arr,j+1,right)

            // pivot = arr[right]
            // while(i<j) {
            //   while (i<j && arr[i] >= pivot) {
            //     i++;
            //   }
            //   while (i<j && arr[j] <= pivot) {
            //     j--;
            //   }
            //   [arr[i],arr[j]] = [arr[j],arr[i]]
            // }
            // arr[right] = arr[i];
            // arr[i] = pivot;
            // this.quickSort_descending(arr,left,i-1)
            // this.quickSort_descending(arr,i+1,right)


            return arr;


          }
```

