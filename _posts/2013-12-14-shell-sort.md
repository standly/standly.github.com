---
layout: post
title: 希尔排序，C语言数组
category: 算法
tags: 算法 Algorithm 希尔排序 Shell sort，array
---

希尔排序是直接插入排序的改进版，它主要通过选择合适的插入步长来解决问题。  
也就是说他每隔n位选择元素来比较，并插入。然后逐步的缩小n至1.这样就解决局部有序的序列，移动过多的弊端。  
而且它能够快速解决类似'|8|1|2|3|4|0|'的排序问题。如果步长合适的话，一次就可以解决问题。  

算法实现如下：

	void ShellSort()
	{
		int i,j,temp,k;
		int gap[]={1750,701,301,132,57,23,10,4,1};
		int *pGap=gap;
		
		while(pGap[0]>=1&&pGap[0]<arrayLength)//遍历至小于数组长度的第一个gap 
		{
			k=pGap[0];
			for(j=k;j<arrayLength;j+=k)//对以k为长度的数组下标组成的k的子集进行插入排序 
			{
				temp=a[j];
				for(i=j;i>=0&&temp<a[i-k];i-=k)
				{
					a[i]=a[i-k];				
				}
				a[i]=temp;
			}
			pGap++;
		}
	} 