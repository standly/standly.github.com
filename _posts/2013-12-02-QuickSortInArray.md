---
layout: post
title: QuickSort 快排(以int数组为例)
category: 算法
tags: 算法 Algorithm
---

最近在复习算法。主要就是回想一下基本过程，然后用C实现它。  
但是在复习快排算法的时候，我又一次的失败了。实现总是有Bug。

###快排算法的实现

还是先回忆一下基本原理或者说执行过程吧！  
假设甲站在一个队列里，队列不是按照高矮顺序排列的。然后甲说比我高的请站在我前面，比我矮的请站在我后面。  
这样队列就分为了前后两个队列，然后这两个队列再重复上述过程，最后整个队列就会有序了。  
当然程序里面执行起来当然更机械一点，但大致过程就是这样的。  
然后我在代码实现过程中就感觉有点难以入手。  
以下是我第一份代码，当然是错误的代码：

	void QSort(int start,int end ,int *a)
	{
		if(start==end){ return; }
		int i=start,j;
		j=start;//选择a[start]为基准值 
		for(;i<end;i++)
		{
			if(a[i]>a[j])//如果大于基准则交换
			{
				Swap(a+i,a+j);
				j=i;
			}
		 }
		 QSort(start,j-1,a);
		 QSort(j+1,end,a);
	}


现在看起来我在写这份代码的时候明显心里还想着冒泡。在这里:

	if(a[i]>a[j])//如果大于基准则交换
	{
		Swap(a+i,a+j);
		j=i;
	}
		
应该把比基准值小的放到一边，比基准值大的放到另一半，但是却只实现了一半。  
所以还剩一半。改进后：

	void QSort(int start,int end ,int *a)
	{
		if(start>=end){ return; }
		int i=start,j=end,k;
		k=start;//选择a[start]为基准值 
		for(;i<j;)
		{
			if(a[i]>a[k])//将比基准值小的放到前面 
			{
				Swap(a+i,a+k);
				k=i;
				i++;
			}
			else//将比基准值大的放到后面 
			{
				Swap(a+i,a+j);
				j--;
			}
		 }
		 QSort(start,j-1,a);
		 QSort(j+1,end,a);
	}

但是依然有问题。在

	if(a[i]>a[k])//将比基准值小的放到前面 
	 	{
	 		Swap(a+i,a+k);
	 		k=i;
	 		i++;
	 	}
		
这里，将a[i],a[k]互换之后，i到k之间有可能会遗漏数据。  
所以再次更改。更改后为：

	void QSort(int start,int end ,int *a)
	{
		if(start>=end){ return; }
		int i=start,j=end,base=start;//选择a[start]为基准值 
		for(;i<j;)
		{
			if(a[i]>base)//将比基准值大的放到后面 
			{
				Swap(a+i,a+j);
				j=j-1;
			}
			if(a[j]<base)//将比基准值小的放到前面 
			{
				Swap(a+i,a+j);
				i=i+1;
			}
		 }
		 QSort(start,j-1,a);
		 QSort(j+1,end,a);
	}

依然有错误！！！  
依然是基准值的问题。a[start]定为了基准值，然后从a[start]开始比较。此算法使得base值的index只会发生一次改变。  
然后再重新思考问题。我需要做的只是将队列分为两个部分，一部分都小于base，另一部分都大于base。  
以如下数组为例：
	|7|4|9|1|5|3|4|6|2|0|

若以首元素7为基准值，则拿出7，剩余的数组就变为：
	|_|4|9|1|5|3|4|6|2|0|

然后将数组分为两部分。大于7向右移，反之左移。  
移动过程就好想象了，从两头开始向中间遍历，移动低于或者高于基准值的元素，然后插入空白的地方。  
因为空位在前面，前面需要插入小元素，所以先从后遍历，比如:

	j右←左：找到第一个比7小的数0，放到a[0]的位置，那么a[length-1]就空下来了。当然如果比7大就不需要移动。  
	|0|4|9|1|5|3|4|6|2|_|
	i左→右:找到第一个比7大的数9，所以放到a[0]的位置，那么现在a[1]就空下来了。   
	|0|4|_|1|5|3|4|6|2|9|


然后如此反复即可。  
然后代码如下

	void QuickSort(int start,int end ,int *a)
	{
		if (start < end)
		{
			//Swap(a[start],a[randNumberBetweenSatrtAndEnd]);//随机选择基准 
			int i = start, j = end, base = a[start];
			while (i < j)
			{
				while(i < j && a[j] >= base) // 从右向左找第一个小于base的数
					j--; 
				if(i < j)
					a[i++] = a[j];
						
				while(i < j && a[i] < base) // 从左向右找第一个大于等于base的数
					i++; 
				if(i < j)
					a[j--] = a[i];
			}
			a[i] = base;
			QuickSort( start, i - 1,a); // 递归调用 
			QuickSort( i + 1, end,a);
		}
	}

这就是最后的代码了。默认是使用头一个元素为基准值，但是我们知道快排的效率就决定于基准值的大小。  
所以要选择合适的基准值，使用随机方式，效率应该比固定开头位置的平均效率要好一点。

###快排算法的性能分析(摘自百度百科)
