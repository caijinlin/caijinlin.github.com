---
layout: post
title: "五子棋c语言算法实现"
description: "五子棋"
keywords: "五子棋"
category: 游戏
tags: [五子棋]
---
{% include JB/setup %}

### 五子棋

五子棋是一种两人对弈的纯策略型汉族棋类益智游戏，棋具与围棋通用，由中国古代汉族人发明，起源于中国上古时代的传统黑白棋种之一。主要流行于华人和汉字文化圈的国家以及欧美一些地区。容易上手，老少皆宜，而且趣味横生，引人入胜

<!-- more -->

判断输赢：以某一点为中心向四周4个方向判断，连续数达到5个即取胜

源代码实现c算法版：
    	
    #include<stdio.h>
    #include<memory.h>
    #define size 10 //棋盘大小
    char map[size][size]= {};
    int row = 0,col = 0;
    /*初始化棋盘数组*/
    void init()
    {
        memset(map,' ',sizeof(map));
    }
    /*显示棋盘*/
    void showmap()
    {
        int loop=0,row=0,col=0;
        for(loop=0;loop<size;loop++)
        {
            printf("%4d",loop+1);
        }
        printf("\n");
        for(row=0;row<size;row++)
        {
            printf("  ");
            for(loop=0;loop<size;loop++)
            {
                printf("---+");
            }
            printf("\n");
            printf("%2d|",row+1);
            for(col=0;col<size;col++)
            {
                printf("%c | ",map[row][col]);
            }
            printf("\n");
        }
        printf("  ");
        for(loop=0;loop<size;loop++)
        {
            printf("---+");
        }
        printf("\n");
    }
    /*判断输赢,胜利则返回玩胜利玩家*/
    char judge(int x, int y, char player)
    {
    int count=0;//记录是否组成五子棋

    /*东北和西南方向判断开始*/
    /*向右上45度(东北方向)判断*/
    row = x-1;
    col = y+1;
    while((row>=0)&&(col<=size-1))
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        row--;
        col++;
    }
    /*向左下45度(西南方向)判断*/
    row = x+1;
    col = y-1;
    while((row<=size-1)&&(col>=0))
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        row++;
        col--;
    }
    if(count+1==5)
    {
        return player;
    }
    /*东北和西南方向判断结束*/

    /*西北和东南方向方向判断开始*/
    /*向左上45度(西北方向)判断*/
    count = 0;
    row = x-1;
    col = y-1;
    while((row>=0)&&(col>=0))
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        row--;
        col--;
    }
    /*向右下45度(东南方向)判断*/
    row = x+1;
    col = y+1;
    while((row<=size-1)&&(col<=size-1))
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        row++;
        col++;
    }
    if(count+1==5)
    {
        return player;
    }
    /*西北和东南方向判断结束*/

    /*竖直方向方向判断开始*/
    /*竖直向上判断*/
    count = 0;
    row = x-1;
    col = y;
    while((row>=0))
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        row--;
    }
    /*竖直向下判断*/
    row = x+1;
    col = y;
    while((row<=size-1))
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        row++;
    }
    if(count+1==5)
    {
        return player;
    }
    /*竖直方向判断结束*/

    /*水平方向判断*/
    /*水平向左判读*/
    count = 0;
    row = x;
    col = y-1;
    while(col>=0)
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        col--;
    }
    /*水平向右判断*/
    row = x;
    col = y+1;
    while((col<=size-1))
    {
        if(map[row][col]==player)
        {
            count++;
        }
        else
        {
            break;
        }
        col++;
    }
    if(count+1==5)
    {
        return player;
    }
    /*水平方向判断结束*/
    return '0';//没有形成五子棋时
    }
    /*下棋*/
    void chess()
    {
    char player = 'A';//玩家
    char winner = '0';//赢家
    while(winner=='0')
    {
    do
    {
         printf("玩家%c要下棋的位置坐标:",player);
         scanf("%d,%d",&row,&col);
         printf("坐标为:%d,%d\n",row,col);
         printf("%s",(map[row-1][col-1]!=' ')?"输入无效,请重新输入\n":"输入正确\n");
    }while(map[row-1][col-1]!=' ');
    map[row-1][col-1]=player;
    showmap();
    winner = judge(row-1,col-1,player);//玩家下棋后，马上进行判断输赢
    player = (player=='A')?'B':'A';//切换玩家
    }
    printf("游戏结束，玩家%c赢\n",winner);
    }
    int main()
    {
    init();     /*初始化棋盘位置元素值*/
    showmap(); /*第一次显示棋盘*/
    chess();  /*开始下棋*/

    }