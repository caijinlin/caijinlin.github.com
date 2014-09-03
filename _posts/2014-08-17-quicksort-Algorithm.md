---
layout: post
title: "快速排序代码总结"
description: "排序算法"
keywords: "排序算法"
category: 算法
tags: [快速排序]
---
{% include JB/setup %}

### 快速排序

#### 快速排序的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

<!-- more -->

####不需要枢轴的快排流程图（最简单的一种）

![蔡金林的博客](/assets/images/quicksort.png)

####快速排序

	/*快速排序思想:分治法*/
	class Mysort
	{
	    public:
	    void output(int *a, int n)
	    {
	        int loop = 0;
	        for(;loop<n;loop++)
	        {
	            cout<<a[loop]<<" ";
	        }
	        cout<<endl;
	    }
	    void swap(int *m, int *n)
	    {
	        int temp;
	        temp = *m;
	        *m = *n;
	        *n = temp;
	    }
	    /*分组*/
	    int  partition(int *a, int left, int right)
	    {
	        /*设置为key=a[right]*/
	        int i=left-1, p = right;//设置枢轴，也可以设置p=right;
	        int j = left;
	        int key = a[p];
	        for(;j<right;j++)
	        {
	            if(a[j]<=key)
	            {
	                i++;//移动位置
	                swap(&a[i],&a[j]);
	            }
	        }
	        swap(&a[p],&a[i+1]);
	        return i+1;

	        /*设置枢轴，也可以设置p=left;
	        int i=left, p = left;
	        int j = left+1;
	        int key = a[p];
	        for(;j<right;j++)
	        {
	            if(a[j]<=key)
	            {
	                i++;//移动位置
	                swap(&a[i],&a[j]);
	            }
	        }
	        swap(&a[p],&a[i]);
	        return i;
	        */
	    }
	    /*元素个数快速*/
	    void quicksort1(int *a, int n) //数组，元素个数
	    {
	        int left = 0;
	        int right = n-1;
	        if(n<=1)
	        {
	            return ;
	        }
	        while(left<right)
	        {
	            while(a[left]<a[right]&&left<right)
	            {
	                left++;
	            }
	            swap(&a[left],&a[right]);
	            while(a[right]>a[left]&&left<right)
	            {
	                right--;
	            }
	            swap(&a[left],&a[right]);
	        }
	        quicksort1(a,left);
	        quicksort1(a+left+1,n-left-1);//参数为元素个数时，指针需要移动
	    }
	    /*下标快速*/
	    void quicksort2(int *a, int left, int right)
	    {
	        if(left<right)
	        {
	             while(left<right)
	             {
	                while(a[left]<a[right]&&left<right)
	                {
	                    left++;
	                }
	                swap(&a[left],&a[right]);
	                while(a[right]>a[left]&&left<right)
	                {
	                    right--;
	                }
	                swap(&a[left],&a[right]);
	            }
	            quicksort2(a,0,left-1);
	            quicksort2(a,left+1,right);//参数为下标，指针不需要移动
	        }
	    }

	    /*分组快排*/
	    void quicksort3(int *a, int left, int right)
	    {
	        int q = 0;
	        if(left<right)
	        {
	            q = partition(a,left,right);
	            quicksort3(a,left,q-1);
	            quicksort3(a,q+1,right);
	        }
	    }

	    /*以中间值作为枢纽进行快排*/
	    void quicksort4(int *a, int left,  int right)
	    {
	        int mid, i, j;
	        if(left<right)
	        {
	                mid = (left+right)/2;
	                i=left-1;
	                j=right+1;
	                while(1)
	                {
	                    while(a[++i]<a[mid]);
	                    while(a[--j]>a[mid]);
	                    if(i>=j)
	                    {
	                    	break;
	                    }
	                    swap(&a[i],&a[j]);
	                }
	                quicksort4(a,left,i-1);
	                quicksort4(a,j+1,right);
	        }

	    }
	};

