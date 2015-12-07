---
layout: post
title: '网络流学习'
date: 2015-03-18 08:19
comments: true
categories: 
---
### 原本应该学数据结构的。。结果被赶着去学网络流了。。。
---
### upd 2015.3.23 终于切完了，该坑完结，撒花~~
<!--more-->
---
# #1
#### 题目描述
>第二次世界大战时期，英国皇家空军从沦陷国征募了大量外籍飞行员。由皇家空军派出的每一架飞机都需要配备在航行技能和语言上能互相配合的2 名飞行员，其中1 名是英国飞行员，另1 名是外籍飞行员。在众多的飞行员中，每一名外籍飞行员都可以与其他若干名英国飞行员很好地配合。如何选择配对飞行的飞行员才能使一次派出最多的飞机。对于给定的外籍飞行员与英国飞行员的配合情况，试设计一个算法找出最佳飞行员配对方案，使皇家空军一次能派出最多的飞机。
对于给定的外籍飞行员与英国飞行员的配合情况，编程找出一个最佳飞行员配对方案，使皇家空军一次能派出最多的飞机。

####题解
在左边搞个源点，右边搞个汇点
然后源点和左半个图，汇点和右半个图之间连上流量为1的边
左半个图和右半个图之间连上流量为maxlongint的边
就可以了。。
不知道为什么我的c++总是A不了，然后我之前写的pascal过了，然后我现在看那个pascal是错误百出的。。
算了，能过就好，反正会建图。。
```delphi

const shuru='air.in';
      shuchu='air.out';
var	next,t,sap:array[0..2003] of longint;
	queue,d,cur,headlist:array[0..203] of longint;
	vis:array[0..203] of boolean;
	m,front,finish,num,x,y,i,j,k,n,ans:longint;
procedure init;
begin	
	assign(input,shuru);
	assign(output,shuchu);
	reset(input);
	rewrite(output);
	readln(m,n);
	for i:=1 to m+n+2 do headlist[i]:=-1;
	readln(X,y);
	num:=-1;
	while (x<>-1) and (y<>-1) do
	begin
		inc(num);
		next[num]:=headlist[x];
		headlist[x]:=num;
		t[num]:=y;
		sap[num]:=maxlongint;
		inc(num);
		next[num]:=headlist[y];
		headlist[y]:=num;
		t[num]:=x;
		sap[num]:=0;
		readln(x,y);
	end;
	close(input);
	for i:=1 to m do
	begin
		inc(num);
		next[num]:=headlist[n+m+1];
		headlist[n+m+1]:=num;
		t[num]:=i;
		sap[num]:=1;
		inc(num);
		next[num]:=headlist[i];
		headlist[i]:=num;
		t[num]:=n+m+1;
		sap[num]:=0;
	end;	
	for i:=m+1 to m+n do
	begin
		inc(num);
		next[num]:=headlist[i];
		headlist[i]:=num;
		t[num]:=n+m+2;
		sap[num]:=1;
		inc(num);
		next[num]:=headlist[n+m+2];
		headlist[n+m+2]:=num;
		t[num]:=i;
		sap[num]:=0;
	end;
end;
function BFS:boolean;
var i:longint;
begin
	fillchar(vis,sizeof(Vis),false);
	vis[n+m+1]:=true; queue[1]:=n+m+1; front:=1; finish:=1;
	while finish>=front do
	begin
		i:=headlist[queue[front]];
		while i<>-1 do
		begin
			if not(vis[t[i]]) and (sap[i]>0) then begin
						vis[t[i]]:=true;
						inc(finish);
						queue[finish]:=t[i];
						d[t[i]]:=d[queue[front]]+1;
												  end;
			i:=next[i];
		end;
		inc(Front);
	end;
	exit(vis[n+M+2]);
end;
function min(A,b:longint):longint;
begin
	if a<b then exit(A);
	exit(b);
end;
function DFS(x,a:longint):longint;
var i,flow,f:longint;
begin
	if (x=n+m+2) or (A=0) then exit(a);
	flow:=0;
	i:=cur[x];
	while i<>-1 do
	begin
		if d[t[i]]=d[x]+1 then begin
			f:=dfs(t[i],min(a,sap[i]));
			if f>0 then begin
							flow:=flow+f;
							sap[i]:=sap[i]-f;
							sap[i xor 1]:=sap[i xor 1]+f;
							a:=a-f;
							if a=0 then break;
						end;
							  end;
		i:=next[i];
		cur[x]:=i;
	end;
	if flow=0 then d[x]:=-1;
	exit(flow);
end;
procedure main;
begin
	init;
	while BFS do
	begin
		for i:=1 to n+m+2 do cur[i]:=headlist[i];
		ans:=ans+dfs(n+m+1,maxlongint);
	end;
	writeln(Ans);
	close(output);
end;
begin
	main;
end.

```

---
# #2

#### 题目描述
>W 教授正在为国家航天中心计划一系列的太空飞行。每次太空飞行可进行一系列商业
性实验而获取利润。现已确定了一个可供选择的实验集合E={E1，E2，…，Em}，和进行这
些实验需要使用的全部仪器的集合I={I1，I2，…In}。实验Ej需要用到的仪器是I的子集RjÍI。
配置仪器Ik的费用为ck美元。实验Ej的赞助商已同意为该实验结果支付pj美元。W教授的
任务是找出一个有效算法，确定在一次太空飞行中要进行哪些实验并因此而配置哪些仪器才
能使太空飞行的净收益最大。这里净收益是指进行实验所获得的全部收入与配置仪器的全部
费用的差额。

#### 题解
该题就是求最大权闭合子图。
对于最大权闭合子图，我们把S和所有权值大于0的点连边，流量为权值，把T和所有权值小于0的点连边，流量为权值的绝对值。如果原来(a,b)有边，就对a,b连边，流量为inf,对其求最大流，最终答案为所有权值大于0的点的和减去最大流。
证明：
    该题的最大流就是它的最小割。又因为最小割肯定是简单割（即不包含流量为inf的边），那么割的边都是S，T连出的边。我们发现因为它不连通，左边没被割掉的边的所有后继都在右边被割掉的边，即{左边没被割掉的边，右边被割掉的边}是一个合法的权闭合子图，tot-maxflow就是它的最大值
    
```c++

#include<iostream>
#include<cstdio>
#include<cstring>
#define fortodo(a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=205,inf=1<< 29;

int a[maxn][maxn],cur[maxn],pre[maxn],num[maxn],d[maxn],his[maxn],n,m,s,t,tot=0;

inline void addedge(int fromm,int too,int value){
	a[fromm][too]+=value;
}

inline void read(){
	
	memset(a,0,sizeof(a));
	int x;
	scanf("%d%d",&m,&n);s=m+n+1;t=m+n+2;
	fortodo(1,m){
		scanf("%d",&x);tot+=x;
		addedge(s,i,x);
		char ch='a';
		while(ch!='\n'){
			scanf("%d",&x);
			addedge(i,x+m,inf);
			ch=getchar();
//			ch=cin.peek();
		}
	}
	fortodo(1,n){
		scanf("%d",&x);
		addedge(i+m,t,x);
	}
}
inline int isap(){
	int ans=0,jl;
	num[0]=n+m+2;
	fortodo(1,n+m+2) cur[i]=1;
	int i=s;
	bool flag;
	int aug=inf,tmp;
	while (d[s]<n+m+2){
		his[i]=aug;
		flag=false;
		for (int j=cur[i];j<=n+m+2;j++){
			if ((a[i][j]>0)&&(d[j]+1==d[i])){
				flag=true;
				cur[i]=j;
				aug=min(aug,a[i][j]);
				pre[j]=i;
				i=j;
				if (i==t){
					ans+=aug;
					while (i!=s){
						tmp=i;
						i=pre[i];
						a[i][tmp]-=aug;
						a[tmp][i]+=aug;
					}
					aug=inf;
				}
				break;
			}
		}
		if (flag) continue;
		int minnum=n+m+1;
		for (int j=1;j<=n+m+2;j++) if ((a[i][j]>0)&&(d[j]<minnum)) {jl=j; minnum=d[j];}
		cur[i]=jl;
		num[d[i]]--;
		if (num[d[i]]==0) return ans;
		d[i]=minnum+1;
		num[d[i]]++;
		if (i!=s){
			i=pre[i];
			aug=his[i];
		}
	}
	return ans;
}

void judge(){
	 freopen("in.txt","r",stdin);
	 freopen("out.txt","w",stdout);
 }
 
 int main(){
 	 judge();
	 read();
	 cout<<tot-isap();
	 return 0;
}

```
---
# #3
#### 题目描述
> 给定有向图G=(V,E)。设P 是G 的一个简单路（顶点不相交）的集合。如果V 中每个
顶点恰好在P 的一条路上，则称P是G 的一个路径覆盖。P 中路径可以从V 的任何一个顶
点开始，长度也是任意的，特别地，可以为0。G 的最小路径覆盖是G 的所含路径条数最少
的路径覆盖。
设计一个有效算法求一个有向无环图G 的最小路径覆盖。

#### 题解
将图中每一个点进行拆点。对于一条有向边< a,b>，将< a,1>与< b,2>相连，这样我们得到一个二分图。我们先将最小路径覆盖设为每一个点都为一个路径，然后不断向其中加入边使得两个路径相连成为一个路径，那么我们只需要求这个二分图的最大匹配即可，答案就是（总点数-最大匹配）
```c++
#include<iostream>
#include<cstdio>
#define fortodo(a,b) for (int i=(a);i<=(b);i++)
#define read2(a,b) scanf("%d%d",&a,&b);
#define read(a) scanf("%d",&a);
#define write(a) printf("%d",a);
using namespace std;
const int maxn=405,maxe=20005,inf=1<<29;
int ne[maxe],head[maxn],sap[maxe],t[maxe],num,n,m,s,tt,pre[maxn],his[maxn],d[maxn],cur[maxn],count[maxn],edge[maxn];
inline void addedge(int fromm,int too,int value){
	ne[num]=head[fromm];
	head[fromm]=num;
	t[num]=too;
	sap[num++]=value;
	ne[num]=head[too];
	head[too]=num;
	t[num]=fromm;
	sap[num++]=0;
}
inline void init(){
	read2(n,m);s=2*n+1;tt=2*n+2;
	int x,y;
	fortodo(1,2*n+2) head[i]=-1;
	fortodo(1,m){
		read2(x,y);
		addedge(x,n+y,inf);
	}
	fortodo(1,n){
		addedge(s,i,1);
		addedge(i+n,tt,1);
	}
}

inline int isap(){
	int i=s,ans=0,jl,minnum,aug=inf;
	fortodo(1,2*n+2) cur[i]=head[i];
	count[0]=2*n+2;
	while (d[s]<2*n+2){
		his[i]=aug;
		bool flag=false;
		for (int j=cur[i];j!=-1;j=ne[j]){
			if ((sap[j]>0)&&(d[t[j]]+1==d[i])){
				flag=true;
				pre[t[j]]=i;
				cur[i]=j;
				aug=min(aug,sap[j]);
				edge[i]=j;
				i=t[j];
				if (i==tt){
					ans+=aug;
					while (i!=s){
						i=pre[i];
						sap[edge[i]]-=aug;
						sap[edge[i]^1]+=aug;
					}
					aug=inf;
				}
				break;
			}
		}
		if (flag) continue;
		minnum=n+1;
		for (int j=head[i];j!=-1;j=ne[j]) if ((sap[j]>0)&&(d[t[j]]<minnum)){minnum=d[t[j]];jl=j;}
		count[d[i]]--;
		if (count[d[i]]==0) return ans;
		d[i]=minnum+1;
		count[d[i]]++;
		cur[i]=jl;
		if (i!=s){i=pre[i];aug=his[i];}
	}
	return ans;
}
inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
 }

int main(){
	judge();
	init();
	cout<<(n-isap());
	return 0;
}


```

---

# #4
#### 题目描述
>假设有n根柱子，现要按下述规则在这n根柱子中依次放入编号为1，2，3，4的球。
（1）每次只能在某根柱子的最上面放球。
（2）在同一根柱子中，任何2个相邻球的编号之和为完全平方数。
试设计一个算法，计算出在n根柱子上最多能放多少个球。例如，在4 根柱子上最多可放11 个球。

#### 题解
我们从小到大枚举答案，如果两个球编号之和为完方数，那么我们就将其连边。然后我们求该图的最小路径覆盖（即上一题）。如果其答案< n就接着枚举，如果>n就break并输出该答案-1。不必采用二分。因为每次加了节点之后可以直接在残余网络上跑。

```c++
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=5005,maxe=2000005,inf=1<<29;
int cur[maxn],head[maxn],t[maxe],sap[maxe],pre[maxn],his[maxn],edge[maxn],ne[maxe],n,flow,S,T,num,count[maxn],d[maxn];

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
 }

inline void addedge(int fromm,int too,int value){
	ne[num]=head[fromm];
	head[fromm]=num;
	sap[num]=value;
	t[num++]=too;
	ne[num]=head[too];
	head[too]=num;
	sap[num]=0;
	t[num++]=fromm;
}

inline void buildmap(){
	fortodo(i,1,n-1)
		fortodo(j,i+1,n)
			if (((floor(sqrt(i+j)))*(floor(sqrt(i+j))))==(i+j)) addedge((i<<1),(j<<1)+1,inf);
	fortodo(i,2,2*n+1){
		if ((i&1)==0)
			addedge(S,i,1);
		else
			addedge(i,T,1);
	}
}

inline void isap(int tot){
	int i=S,aug=inf,jl;
	memset(d,0,sizeof(d));
	count[0]=tot;
	while (d[S]<tot){
		bool flag=false;
		his[i]=aug;
		for (int j=cur[i];j!=-1;j=ne[j])
			if ((d[t[j]]+1==d[i])&&(sap[j]>0)){
				flag=true;
				pre[t[j]]=i;
				edge[i]=j;
				cur[i]=j;
				aug=min(aug,sap[j]);
				i=t[j];
				if (i==T){
					flow+=aug;
					while (i!=S){
						i=pre[i];
						sap[edge[i]]-=aug;
						sap[edge[i]^1]+=aug;
					}
					aug=inf;
				}
				break;
			}
		if (flag) continue;
		int minnum=tot-1;
		for(int j=head[i];j!=-1;j=ne[j]) if ((sap[j]>0)&&(d[t[j]]<minnum)) {minnum=d[t[j]];jl=j;}
		count[d[i]]--;
		if (count[d[i]]==0) return;
		d[i]=minnum+1;
		count[d[i]]++;
		cur[i]=jl;
		if (i!=S){i=pre[i];aug=his[i];}
	}
}

int main(){
	judge();
	int thisans,ans;
	scanf("%d",&n);
	S=1;T=maxn-1;
	fortodo(i,0,maxn-1) head[i]=-1;
	count[0]=2*n+2;
	buildmap();
	fortodo(i,0,maxn-1) cur[i]=head[i];
	isap(count[0]);
	fortodo(i,n+1,inf){
 		addedge(S,2*i,1);
		addedge(2*i+1,T,1);
		fortodo(j,1,i-1) if (((floor(sqrt(i+j)))*(floor(sqrt(i+j))))==(i+j)) addedge((j<<1),(i<<1)+1,inf);
		cur[2*i]=head[2*i];
		cur[2*i+1]=head[2*i+1];
		fortodo(j,0,2*i+1) cur[j]=head[j];
		cur[T]=head[T];
		isap(2*i+2);
		thisans=i-flow;
		if (thisans>n) {  ans=i-1; break;}
	}
	cout<<ans;
	return 0;
}
```
---
# #5
#### 题目描述
> 假设有来自n 个不同单位的代表参加一次国际会议。每个单位的代表数分别为人r1,r2...会议餐厅共有 m张餐桌，每张餐桌可容纳c1,c2..  个代表就餐。
为了使代表们充分交流，希望从同一个单位来的代表不在同一个餐桌就餐。试设计一个算法，
给出满足要求的代表就餐方案。

#### 题解
该题是多重匹配。对于每一个单位，连一条从源到它流量为ri的边。对于每一个餐桌，连一条到汇流量为ci的边，对于每一个单位和餐桌，连一条流量为1的边，跑其最大流即可

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=505,maxe=50005,inf=1<<29;

int num,pre[maxn],head[maxn],ne[maxe],sap[maxe],t[maxe],S,T,his[maxn],edge[maxn],x,n,m,countnum[maxn],cur[maxn],d[maxn];
inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void addedge(int begin,int end,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	t[num++]=begin;
}

inline void init(){
	int x;
	fortodo(i,0,maxn-1) head[i]=-1;
	scanf("%d%d",&m,&n);S=n+m+1;T=n+m+2;
	fortodo(i,1,m){scanf("%d",&x);addedge(S,i,x);}
	fortodo(i,1,n){scanf("%d",&x);addedge(i+m,T,x);}
	fortodo(i,1,m) fortodo(j,1,n) addedge(i,j+m,1);
}

inline int isap(){
	fortodo(i,1,n+m+2) cur[i]=head[i];
	int minnum,i=S,aug=inf,ans=0,jl;
	memset(d,0,sizeof(d));
	countnum[0]=n+m+2;
	while (d[S]<n+m+2){
		bool flag=false;
		his[i]=aug;
		for (int j=cur[i];j!=-1;j=ne[j])
			if ((sap[j]>0)&&(d[t[j]]+1==d[i])){
				flag=true;
				pre[t[j]]=i;
				edge[i]=j;
				aug=min(aug,sap[j]);
				cur[i]=j;
				i=t[j];
				if (i==T){
					ans+=aug;
					while (i!=S){
						i=pre[i];
						sap[edge[i]]-=aug;
						sap[edge[i]^1]+=aug;
					}
					aug=inf;
				}
				break;
			}
		if (flag) continue;
		minnum=n+m+1;
		for (int j=head[i];j!=-1;j=ne[j]) if ((sap[j]>0)&&(d[t[j]]<minnum)){minnum=d[t[j]];jl=j;}
		countnum[d[i]]--;
		if (countnum[d[i]]==0) return ans;
		d[i]=minnum+1;
		countnum[d[i]]++;
		cur[i]=jl;
		if (i!=S){i=pre[i];aug=his[i];}
	}
	return ans;
}

int main(){
	judge();
	init();
	cout<<isap();
	return 0;
}
```
---
# #6
#### 题目描述
> 给定正整数序列x 1 , xn  。
（1）计算其最长递增子序列的长度s。
（2）计算从给定的序列中最多可取出多少个长度为s的递增子序列。
（3）如果允许在取出的序列中多次使用x1和xn，则从给定序列中最多可取出多少个长度为s的递增子序列。

#### 题解
先用dp算出其最长递增子序列s
然后为了限制每个点只能用一次，将其拆成 （i,1）, （i,2）两个点，两个点之间连流量为1的边。
对于d[i]=s的点i，连S到其流量为inf的边。对于d[i]=i的点，连到T流量为inf的边，对于(a[i]< a[j])&&(d[i]=d[j]+1),连i到j流量为1的边。
这样跑出来是第(2)问
把1,n的流量限制改为inf,跑出来是第(3)问

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=1005,maxe=120001,inf=1<<29;

int d[maxn],cur[maxn],num,a[maxn],f[maxn],n,s,head[maxn],ne[maxe],pre[maxn],his[maxn],sap[maxe],t[maxe],edge[maxn],S,T,countnum[maxn];
inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void addedge(int begin,int end,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	t[num++]=begin;
}

inline void init(){
	scanf("%d",&n);S=1;T=2*n+2;
	fortodo(i,1,n) scanf("%d",&a[i]);
	fortodo(i,1,n) f[i]=1;
	for(int i=n;i>=1;i--) for(int j=i+1;j<=n;j++)
	 if (a[i]<a[j]) f[i]=max(f[i],f[j]+1);
	fortodo(i,1,n) s=max(s,f[i]);
}

inline void buildmap(){
	fortodo(i,0,maxn-1) head[i]=-1;
	fortodo(i,1,n){
		if (f[i]==s) addedge(S,2*i,1);
		if (f[i]==1) addedge(2*i+1,T,1);
		addedge(2*i,2*i+1,1);
	}
	for(int i=n-1;i>=1;i--) fortodo(j,i+1,n) if ((a[i]<a[j])&&(f[i]==f[j]+1)) addedge(2*i+1,2*j,1);
}

inline void buildmap2(){
	memset(t,0,sizeof(t));
	memset(sap,0,sizeof(sap));
	memset(ne,0,sizeof(ne));
	num=0;
	fortodo(i,0,maxn-1) head[i]=-1;
	fortodo(i,2,n-1){
		if (f[i]==s) addedge(S,2*i,1);
		if (f[i]==1) addedge(2*i+1,T,1);
		addedge(2*i,2*i+1,1);
	}
	if (f[1]==s) addedge(S,2,inf);
	if (f[1]==1) addedge(3,T,inf);
	addedge(2,3,inf);
	if (f[n]==1) addedge(2*n+1,T,inf);
	addedge(2*n,2*n+1,inf);
	fortodo(i,1,n-1) fortodo(j,i+1,n) if ((a[i]<a[j])&&(f[i]==f[j]+1)) addedge(2*i+1,2*j,1);
}
inline int isap(){
	memset(d,0,sizeof(d));
	memset(countnum,0,sizeof(countnum));
	memset(his,0,sizeof(his));
	fortodo(i,1,2*n+2) cur[i]=head[i];
	countnum[0]=2*n+2;
	int i=S,aug=inf,ans=0,minnum,jl;
	while (d[S]<2*n+2){
		his[i]=aug;
		bool flag=false;
		for (int j=cur[i];j!=-1;j=ne[j])
			if ((sap[j]>0)&&(d[t[j]]+1==d[i])){
				flag=true;
				cur[i]=j;
				pre[t[j]]=i;
				edge[i]=j;
				aug=min(aug,sap[j]);
				i=t[j];
				if (i==T){
					ans+=aug;
					while (i!=S){
						i=pre[i];
						sap[edge[i]]-=aug;
						sap[edge[i]^1]+=aug;
					}
					aug=inf;
				}
				break;
			}
		if (flag) continue;
		minnum=2*n+1;
		for (int j=head[i];j!=-1;j=ne[j]) if ((sap[j]>0)&&(minnum>d[t[j]])){minnum=d[t[j]];jl=j;}
		countnum[d[i]]--;
		if (countnum[d[i]]==0) return ans;
		d[i]=minnum+1;
		cur[i]=jl;
		countnum[d[i]]++;
		if (i!=S){i=pre[i];aug=his[i];}
	}
	return ans;
}
int main(){
	judge();
	init();
	cout<<s<<endl;
	if (s==1){
		cout<<n<<'\n'<<n<<'\n';
		return 0;
	}
	buildmap();
	cout<<isap()<<endl;
	buildmap2();
	cout<<isap()<<endl;
	return 0;
}
```
---
# #7
#### 题目描述
>假设一个试题库中有n道试题。每道试题都标明了所属类别。同一道题可能有多个类别属性，但只能属于一种类别。现在每种类别有其上限，试求最多能拿出多少题目。

#### 题解
sb多重匹配不必多说。。。。
```c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#define fortodo(i,a,b) for(int i=(a);i<=(b);i++)
using namespace std;
const int maxn=2001,maxe=100001,inf=1<<29;;

int S,T,head[maxn],pre[maxn],his[maxn],edge[maxn],t[maxe],countnum[maxn],sap[maxe],ne[maxe],n,num=0,k,cur[maxn],d[maxn];
inline void addedge(int begin,int end,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	t[num++]=begin;
}

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void init(){
	int x,y;
	scanf("%d%d",&k,&n);S=n+k+1;T=n+k+2;
	fortodo(i,1,n+k+2) head[i]=-1;
	fortodo(i,1,k){scanf("%d",&x);addedge(i+n,T,x);}
	fortodo(i,1,n){
		scanf("%d",&x);addedge(S,i,1);
		fortodo(j,1,x){scanf("%d",&y);addedge(i,y+n,1);}
	}
}

inline int isap(){
	fortodo(i,1,n+k+2) cur[i]=head[i];
	int i=S,jl,aug=inf,minnum,ans=0;
	countnum[0]=n+k+2;
	while (d[S]<n+k+2){
		his[i]=aug;
		bool flag=false;
		for(int j=head[i];j!=-1;j=ne[j])
			if ((sap[j]>0)&&(d[t[j]]+1==d[i])){
				flag=true;
				cur[i]=j;
				pre[t[j]]=i;
				edge[i]=j;
				aug=min(aug,sap[j]);
				i=t[j];
				if (i==T){
					ans+=aug;
					while (i!=S){
						i=pre[i];
						sap[edge[i]]-=aug;
						sap[edge[i]^1]+=aug;
					}
					aug=inf;
				}
				break;
			}
		if (flag) continue;
		minnum=n+k+1;
		for(int j=head[i];j!=-1;j=ne[j])if((sap[j]>0)&&(d[t[j]]<minnum)){minnum=d[t[j]];jl=j;}
		countnum[d[i]]--;
		if (countnum[d[i]]==0) return ans;
		d[i]=minnum+1;
		countnum[d[i]]++;
		cur[i]=jl;
		if (i!=S){i=pre[i];aug=his[i];}
	}
	return ans;
}
int main(){
	judge();
	init();
	cout<<isap();
	return 0;
}
```
---
# #8
#### 题目描述
>在一个有m*n 个方格的棋盘中，每个方格中有一个正整数。现要从方格中取数，使任
意2 个数所在方格没有公共边，且取出的数的总和最大。试设计一个满足要求的取数算法。

#### 题解
将方格染色后得到二分图，如果两个方格相邻，则连边。对于每个方格与源（或汇）连边。只要求其最大独立子集即可。易知独立子集一定是覆盖集的补集（略微脑补一下即可），如果我们要求最小覆盖集，我们可以连S到左边流量为其权值的边，右边到T流量为其权值的边，左右之间若有边则连流量为inf的边，然后求其最大流。因为对于某一条增广路，（s,u）,（u,v）,（v,T）一定有一条边被割掉，且不会是（u,v），那么最小割一定会包含（S,u）,（v,T）中一条，那么其中任意一条边都被覆盖了

```c++
#include<iostream>
#include<cstdio>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=921,maxe=6001,inf=1<<29;
 
int m,head[maxn],ne[maxe],t[maxe],sap[maxe],pre[maxn],edge[maxn],d[maxn],his[maxn],countnum[maxn],n,tot,S,T,num,cur[maxn];
 
inline void addedge(int begin,int end,int value){
    ne[num]=head[begin];
    head[begin]=num;
    sap[num]=value;
    t[num++]=end;
    ne[num]=head[end];
    head[end]=num;
    sap[num]=0;
    t[num++]=begin;
}
 
inline int number(int x,int y){
    return (x-1)*n+y;
}
 
inline void init(){
    scanf("%d%d",&m,&n);int x;S=n*m+1;T=S+1;
    fortodo(i,1,T) head[i]=-1;
    fortodo(i,1,m) fortodo(j,1,n){
        scanf("%d",&x);tot+=x;
        if (((i+j)&1)==0){
            addedge(S,number(i,j),x);
            if (i>1) addedge(number(i,j),number(i-1,j),inf);
            if (i<m) addedge(number(i,j),number(i+1,j),inf);
            if (j>1) addedge(number(i,j),number(i,j-1),inf);
            if (j<n) addedge(number(i,j),number(i,j+1),inf);
        }
        else
            addedge(number(i,j),T,x);
    }
}
 
inline int isap(){
    countnum[0]=T;
    fortodo(i,1,T) cur[i]=head[i];
    int i=S,jl,ans=0,minnum,aug=inf;
    while (d[S]<T){
        his[i]=aug;
        bool flag=false;
        for (int j=cur[i];j!=-1;j=ne[j])
            if ((sap[j]>0)&&(d[t[j]]+1==d[i])){
                flag=true;
                edge[i]=j;
                cur[i]=j;
                pre[t[j]]=i;
                aug=min(aug,sap[j]);
                i=t[j];
                if (i==T){
                    ans+=aug;
                    while (i!=S){
                        i=pre[i];
                        sap[edge[i]]-=aug;
                        sap[edge[i]^1]+=aug;
                    }
                    aug=inf;
                }
                break;
            }
        if (flag) continue;
        minnum=T-1;
        for(int j=head[i];j!=-1;j=ne[j]) if ((sap[j]>0)&&(d[t[j]]<minnum)){minnum=d[t[j]];jl=j;}
        countnum[d[i]]--;
        if (countnum[d[i]]==0) return ans;
        d[i]=minnum+1;
        countnum[d[i]]++;
        cur[i]=jl;
        if (i!=S){i=pre[i];aug=his[i];}
    }
    return ans;
}
int main(){
    init();
    cout<<tot-isap();
    return 0;
}
```
---
# #9

#### 题目描述
>一个餐厅在相继的N 天里，每天需用的餐巾数不尽相同。假设第i天需要ri块餐巾(i=1，
2，…，N)。餐厅可以购买新的餐巾，每块餐巾的费用为p分；或者把旧餐巾送到快洗部，
洗一块需m天，其费用为f 分；或者送到慢洗部，洗一块需n 天(n>m)，其费用为s<f 分。
每天结束时，餐厅必须决定将多少块脏的餐巾送到快洗部，多少块餐巾送到慢洗部，以及多
少块保存起来延期送洗。但是每天洗好的餐巾和购买的新餐巾数之和，要满足当天的需求量。
试设计一个算法为餐厅合理地安排好N 天中餐巾使用计划，使总的花费最小。

#### 题解
这个问题的主要约束条件是每天的餐巾够用，而餐巾的来源可能是最新购买，也可能是前几天送洗，今天刚刚洗好的餐巾。每天用完的餐巾可以选择送到快洗部或慢洗部，或者留到下一天再处理。

经过分析可以把每天要用的和用完的分离开处理，建模后就是二分图。二分图X集合中顶点Xi表示第i天用完的餐巾，其数量为ri，所以从S向Xi连接容量为ri的边作为限制。Y集合中每个点Yi则是第i天需要的餐巾，数量为ri，与T连接的边容量作为限制。每天用完的餐巾可以选择留到下一天（Xi->Xi+1），不需要花费，送到快洗部(Xi->Yi+m)，费用为f，送到慢洗部(Xi->Yi+n)，费用为s。每天需要的餐巾除了刚刚洗好的餐巾，还可能是新购买的(S->Yi)，费用为p。

在网络上求出的最小费用最大流，满足了问题的约束条件（因为在这个图上最大流一定可以使与T连接的边全部满流，其他边只要有可行流就满足条件），而且还可以保证总费用最小。

``` c++
#include<iostream>
#include<cstdio>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=3001,maxe=50001,inf=1<<29;

int edge[maxn],pre[maxn],head[maxn],t[maxe],ne[maxe],sap[maxe],cost[maxe],num,d[maxn],Queue[maxe];
bool vis[maxn];
inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

int n,p,m,f,mann,s,a[maxn],S,T,ans;

inline void init(){
	scanf("%d%d%d%d%d%d",&n,&p,&m,&f,&mann,&s);
	S=1;T=2*n+2;
	fortodo(i,1,n) scanf("%d",&a[i]);
	fortodo(i,S,T) head[i]=-1;
}

inline void buildmap(){
	fortodo(i,1,n){addedge(S,i*2,a[i],0);addedge(i*2+1,T,a[i],0);addedge(S,2*i+1,inf,p);}
	fortodo(i,1,n-1) addedge(i*2,(i+1)*2,inf,0);
	fortodo(i,1,n-m) addedge(i*2,(i+m)*2+1,inf,f);
	fortodo(i,1,n-mann) addedge(i*2,(i+mann)*2+1,inf,s);
}

inline bool SPFA(){
	int x;
	fortodo(i,S,T) d[i]=inf;
	d[S]=0;
	int front=0,finish=1;
	Queue[1]=S;
	while (front<finish){
		x=Queue[++front];
		for (int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[t[i]]>d[x]+cost[i])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=true;
				}				
			}
		vis[x]=false;
	}
	return (d[T]!=inf);
}

inline void augment(){
	int aug=inf;
	for (int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for (int i=T;i!=S;i=pre[i]) {ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}

inline void flowcost(){
	while (SPFA()) augment();
}

int main(){
	judge();
	init();
	buildmap();
	flowcost();
	cout<<ans;
	return 0;
}
```
---
# #10
#### 题目描述
>给定一张航空图，图中顶点代表城市，边代表2城市间的直通航线。现要求找出一条满
足下述限制条件的且途经城市最多的旅行路线。
(1)从最西端城市出发，单向从西向东途经若干城市到达最东端城市，然后再单向从东
向西飞回起点(可途经若干城市)。
(2)除起点城市外，任何城市只能访问1次。

#### 题解
不妨将来回飞一圈当成两架飞机一起往右飞。
因为除了起点终点外，任何城市只能访问一次，将其拆点，对于（i,1）,（i,2）连流量为1的边。
如果两城市有航班，则连流量为inf的边。
这样我们就可以构造出一个可行解，可是怎么样才能让经过的城市最多呢？
对于边（i,1）（ i,2），设其费用为1，求最大费用流即可。
```c++
#include<iostream>
#include<cstdio>
#include<string>
#include<map>
#include<cstring>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=301,maxe=10001,inf=1<<29;
int head[maxn],sap[maxe],ne[maxe],t[maxe],cost[maxe],n,v,S,T,Queue[maxe],d[maxn],flow,costans,num,pre[maxn],edge[maxn];
bool vis[maxn],flag=false;
map<string,int> count;

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}

inline void init(){
	scanf("%d%d",&n,&v);S=1;T=2*n;
	fortodo(i,S,T) head[i]=-1;
	string s;
	fortodo(i,1,n){cin>>s;count[s]=i;}
}

inline void buildmap(){
	string s1,s2;int x,y;
	fortodo(i,1,v){
		cin>>s1;cin>>s2;
		x=count[s1];y=count[s2];
		if (x>y) swap(x,y);
    	if((x==1)&&(y==n)) flag=true;
		addedge(2*x,2*y-1,1,0);
	}
	fortodo(i,2,n-1)addedge(2*i-1,2*i,1,-1);
	addedge(1,2,2,-1);
	addedge(2*n-1,2*n,2,-1);
}

inline bool SPFA(){
	int front=0,finish=1;memset(vis,false,sizeof(vis));
	fortodo(i,S,T) d[i]=inf;
	Queue[1]=S;d[1]=0;
	while (front<finish){
		int x=Queue[++front];
		for (int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[x]+cost[i]<d[t[i]])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=true;
				}
			}
		vis[x]=false;
	}
	return d[T]!=inf;
}

inline void Augment(){
	int aug=inf;
	for(int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for(int i=T;i!=S;i=pre[i]) {costans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
	flow+=aug;
}

void CostFlow(){
	while (SPFA()) Augment();
}

int main(){
	judge();
	init();
	buildmap();
	CostFlow();
    if((flow==1)&&(flag)) cout<<2;
	if (flow!=2) cout<<"No Solution!";
				 else cout<<-(costans)-2;
	return 0;
}
```
---
# #11
#### 题目描述
>由于人类对自然资源的消耗，人们意识到大约在2300 年之后，地球就不能再居住了。于是在月球上建立了新的绿地，以便在需要时移民。令人意想不到的是，2177 年冬由于未知的原因，地球环境发生了连锁崩溃，人类必须在最短的时间内迁往月球。现有n个太空站位于地球与月球之间，且有m 艘公共交通太空船在其间来回穿梭。每个太空站可容纳无限多的人，而每艘太空船i 只可容纳H[i]个人。每艘太空船将周期性地停靠一系列的太空站，
例如：(1，3，4)表示该太空船将周期性地停靠太空站134134134…。每一艘太空船从一个太空站驶往任一太空站耗时均为1。人们只能在太空船停靠太空站(或月球、地球)时上、下船。初始时所有人全在地球上，太空船全在初始站。试设计一个算法，找出让所有人尽快地全部转移到月球上的运输方案。

#### 题解

我们把网络优化问题转化为枚举答案+可行性判定问题。枚举天数，按天数把图分层，因为乘船每坐一站天数都要增加一，把太空船航线抽象成图中的一条边，跨图的两层。由于太空船容量有限，边上也要加上容量限制。除了坐船以外，人还可以在某个空间站等待下一班太空船的到来，所以每个点要与下一层同一点连接一条容量为无穷的边。这样在层限制的图上求出的网络最大流就是在当前天数以内能够从地面到月球的最多的人数，该人数随天数递增不递减，存在单调性。所以可以枚举答案或二分答案，用网络流判定。网络流判定问题更适合枚举答案，而不是二分，因为新增一些点和边只需要在原有的基础上增广，不必重新求网络流。

``` c++
#include<iostream>
#include<cstdio>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=1001,maxe=100001,inf=1<<29,maxm=101;
int t[maxe],pre[maxn],edge[maxn],head[maxn],sap[maxe],countnum[maxn],d[maxn],cur[maxn],ne[maxe],num,S=0,T=-1,his[maxn];
int ans=0,hp[maxm],stay[maxm][maxm],staynum[maxm],day=1,n,m,k;
int fa[maxn];

inline int place(int num,int day){
	return (day)*(n+2)+num;
}
inline void addedge(int begin,int end,int flow){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	t[num++]=begin;
}

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void isap(int day){
	int aug=inf,jl,i=S,minnum;
	memset(d,0,sizeof(d));
	memset(countnum,0,sizeof(countnum));
	fortodo(i,0,place(n+1,day)) cur[i]=head[i];
	cur[S]=head[S];cur[T]=head[T];
	countnum[0]=place(n+1,day)+2;
	while (d[S]<place(n+1,day)+2){
		his[i]=aug;
		bool flag=false;
		for (int j=cur[i];j!=-1;j=ne[j])
			if ((sap[j])&&(d[t[j]]+1==d[i])){
				pre[t[j]]=i;
				edge[i]=j;
				aug=min(aug,sap[j]);
				flag=true;
				cur[i]=j;
				i=t[j];
				if (i==T){
					ans+=aug;
					while (i!=S){
						i=pre[i];
						sap[edge[i]]-=aug;
						sap[edge[i]^1]+=aug;
					}
					aug=inf;
				}
				break;
			}
		if (flag) continue;
		minnum=place(n+1,day)+1;
		for (int j=head[i];j!=-1;j=ne[j]) if ((sap[j])&&(d[t[j]]<minnum)){minnum=d[t[j]];jl=j;}
		countnum[d[i]]--;
		if (countnum[d[i]]==0) return;
		d[i]=minnum+1;
		countnum[d[i]]++;
		cur[i]=jl;
		if (i!=S){i=pre[i];aug=his[i];}
	}
}

inline void init(){
	scanf("%d%d%d",&n,&m,&k);
	fortodo(i,0,maxn-1) head[i]=-1;
	fortodo(i,1,m){
		scanf("%d",&hp[i]);
		scanf("%d",&staynum[i]);
		fortodo(j,1,staynum[i]) {
			scanf("%d",&stay[i][j]);
			if (stay[i][j]==-1) stay[i][j]=n+1;
		}
	}
}

inline void addday(int day){
	addedge(S,place(0,day),inf);
	addedge(place(n+1,day),T,inf);
	fortodo(i,0,n+1) addedge(place(i,day-1),place(i,day),inf);
	fortodo(i,1,m){
		int a=stay[i][(day-1+staynum[i])%staynum[i]+1],b=stay[i][(day)%staynum[i]+1];
		addedge(place(a,day-1),place(b,day),hp[i]);
	}
}

inline int find(int n){
	int step=n,x;
	while (fa[step]!=step) step=fa[step];
	while (n!=step){
		x=fa[n];
		fa[n]=step;
		n=x;
	}
	return step;
}

inline void merge(int x,int y){
	int fa1=find(x),fa2=find(y);
	fa[fa2]=fa1;
}

inline int shit(){
	fortodo(i,0,n+1) fa[i]=i;
	fortodo(i,1,m) fortodo(j,2,staynum[i]){
		int x=stay[i][j-1],y=stay[i][j];
		merge(x,y);
	}
	if (find(0)==find(n+1)) return 0;
	return 1;
}

int main(){
	judge();
	init();
	if (shit()){
		printf("0");
		return 0;
	}
	S=maxn-2;T=maxn-1;
	addedge(S,place(0,0),inf);
	addedge(place(n+1,0),T,inf);
	for(day=1;;day++){
		addday(day);
		isap(day);
		if (ans>=k) break;
	}
	cout<<day;
	return 0;
}
```
# #12
#### 题目描述
>给定一个由n 行数字组成的数字梯形如下图所示。梯形的第一行有m 个数字。从梯形
的顶部的m 个数字开始，在每个数字处可以沿左下或右下方向移动，形成一条从梯形的顶
至底的路径。
规则1：从梯形的顶至底的m条路径互不相交。
规则2：从梯形的顶至底的m条路径仅在数字结点处相交。
规则3：从梯形的顶至底的m条路径允许在数字结点相交或边相交。

#### 题解
为了限制每个数字节点只能流一次，拆点，两点直接连流量为1的边即可。
为了限制边不能重合，数字与数字相连时流量为1即可。
剩下的自己加一下费用跑跑费用流即可

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#define sqr(a) (a*a)
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxm=22,maxn=2001,maxe=10001,inf=1<<29;
int a[maxm][maxm],head[maxn],t[maxe],ne[maxe],sap[maxe],cost[maxe],num,S,T,m,n,d[maxn],Queue[maxe],edge[maxn],pre[maxn],ans;
bool vis[maxn];

inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void init(){
	scanf("%d%d",&m,&n);
	fortodo(i,1,n) fortodo(j,1,m+i-1) scanf("%d",&a[i][j]);
}

inline int place(int x,int y){
	return (x-2+m+m)*(x-1)/2+y;
}
inline bool SPFA(){
	memset(vis,0,sizeof(vis));
	fortodo(i,S,T) d[i]=-inf;
	int front=0,finish=1;
	vis[S]=1;Queue[1]=S;d[S]=0;
	while(front<finish){
		int x=Queue[++front];
		for(int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[x]+cost[i]>d[t[i]])){
				d[t[i]]=d[x]+cost[i];
				edge[t[i]]=i;
				pre[t[i]]=x;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=1;
				}
			}
		vis[x]=0;
	}
	return d[T]!=-inf;
}

inline void Augment(){
	int aug=inf;
	for(int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for(int i=T;i!=S;i=pre[i]){ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}

inline int work(){
	ans=0;
	while (SPFA()) Augment();
	return ans;
}
inline void buildmap1(){
	S=0;T=2*place(n,m+n-1)+1;
	fortodo(i,S,T) head[i]=-1;
	fortodo(i,1,m) addedge(S,place(1,i)*2-1,1,0);
	fortodo(i,1,n) fortodo(j,1,m+i-1){
		int x=place(i,j);
		addedge(2*x-1,2*x,1,a[i][j]);
	}
	fortodo(i,1,n-1) fortodo(j,1,m+i-1){
		addedge(2*place(i,j),2*place(i+1,j)-1,1,0);
		addedge(2*place(i,j),2*place(i+1,j+1)-1,1,0);
	}
	fortodo(i,1,m+n-1) addedge(place(n,i)*2,T,1,0);
}
inline void buildmap2(){
	S=0;T=2*place(n,m+n-1)+1;
	fortodo(i,S,T) head[i]=-1;
	fortodo(i,1,m) addedge(S,place(1,i)*2-1,1,0);
	fortodo(i,1,n) fortodo(j,1,m+i-1){
		int x=place(i,j);
		addedge(2*x-1,2*x,inf,a[i][j]);
	}
	fortodo(i,1,n-1) fortodo(j,1,m+i-1){
		addedge(2*place(i,j),2*place(i+1,j)-1,1,0);
		addedge(2*place(i,j),2*place(i+1,j+1)-1,1,0);
	}
	fortodo(i,1,m+n-1) addedge(place(n,i)*2,T,inf,0);
}
inline void buildmap3(){
	S=0;T=2*place(n,m+n-1)+1;
	fortodo(i,S,T) head[i]=-1;
	fortodo(i,1,m) addedge(S,place(1,i)*2-1,1,0);
	fortodo(i,1,n) fortodo(j,1,m+i-1){
		int x=place(i,j);
		addedge(2*x-1,2*x,inf,a[i][j]);
	}
	fortodo(i,1,n-1) fortodo(j,1,m+i-1){
		addedge(2*place(i,j),2*place(i+1,j)-1,inf,0);
		addedge(2*place(i,j),2*place(i+1,j+1)-1,inf,0);
	}
	fortodo(i,1,m+n-1) addedge(place(n,i)*2,T,inf,0);
}
int main(){
	judge();
	init();
	buildmap1();
	cout<<work()<<endl;
	buildmap2();
	cout<<work()<<endl;
	buildmap3();
	cout<<work()<<endl;
	return 0;
}
```
---
# #13
#### 题目描述
> W 公司有m个仓库和n 个零售商店。第i 个仓库有ai个单位的货物；第j 个零售商店需要bj个单位的货物。货物供需平衡，从第i 个仓库运送每单位货物到第j 个零售商店的费用为cij 。试设计一个将仓库中所有货物运送到零售商店的运输方案，使总运输费用最少。

#### 题解
由S向每个仓库i连费用为0，流量为ai的边，由每个商店向T连接费用为0,流量为bj的的边，对于每一个(i,j),连流量为inf,费用为cij的边。跑最小费用流即可

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=305,maxe=60005,inf=1<<29;
int pre[maxn],edge[maxn],n,m,t[maxe],num,head[maxn],ne[maxe],sap[maxe],cost[maxe],d[maxn],Queue[maxe],S,T;
int beifenhead[maxn],beifenne[maxe],beifensap[maxe],beifencost[maxe],beifent[maxe],ans=0;
bool vis[maxn];

inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void init(){
	int x;
	scanf("%d%d",&m,&n);S=n+m+1;T=n+m+2;
	fortodo(i,0,T) head[i]=-1;
	fortodo(i,1,m){scanf("%d",&x);addedge(S,i,x,0);}
	fortodo(i,1,n){scanf("%d",&x);addedge(i+m,T,x,0);}
	fortodo(i,1,m) fortodo(j,1,n){scanf("%d",&x);addedge(i,j+m,inf,x);}
}

inline bool SPFA(){
	int front=0,finish=1;
	memset(vis,false,sizeof(vis));
	fortodo(i,1,T) d[i]=inf;
	Queue[1]=S;vis[S]=true;d[S]=0;
	while (front<finish){
		int x=Queue[++front];
		for (int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[t[i]]>d[x]+cost[i])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=true;
				}
			}
		vis[x]=false;
	}
	return d[T]!=inf;
}

inline void Augment(){
	int aug=inf;
	for (int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for (int i=T;i!=S;i=pre[i]){ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}

int main(){
	init();
	fortodo(i,1,T) beifenhead[i]=head[i];
	fortodo(i,0,num){
		beifent[i]=t[i];
		beifencost[i]=cost[i];
		beifensap[i]=sap[i];
		beifenne[i]=ne[i];
	}
	while (SPFA()) Augment();
	cout<<ans<<endl;ans=0;
	fortodo(i,1,T) head[i]=beifenhead[i];
	fortodo(i,0,num){
		t[i]=beifent[i];
		cost[i]=-beifencost[i];
		sap[i]=beifensap[i];
		ne[i]=beifenne[i];
	}
	while (SPFA()) Augment();
	cout<<-ans;
	return 0;
}
```
---
# #14
#### 题目描述
> 有n件工作要分配给n个人做。第i 个人做第j 件工作产生的效益为才cij。试设计一个将n件工作分配给n个人做的分配方案，使产生的总效益最大。
 
#### 题解
傻逼费用流

```c++
 #include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=105,maxe=20005,inf=1<<29;
int t[maxe],num,head[maxn],sap[maxe],ne[maxe],cost[maxe],pre[maxn],edge[maxn],d[maxn],S,T,n,Queue[maxe],ans=0;
int beifent[maxe],beifenhead[maxn],beifensap[maxe],beifenne[maxe],beifencost[maxe];
bool vis[maxn];

inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}

inline void buildmap(){
	scanf("%d",&n);S=2*n+1;T=2*n+2;int x;
	fortodo(i,1,T) head[i]=-1;
	fortodo(i,1,n) {addedge(S,i,1,0);addedge(i+n,T,1,0);}
	fortodo(i,1,n) fortodo(j,1,n){
		scanf("%d",&x);
		addedge(i,j+n,1,x);
	}
}

inline bool SPFA(){
	int front=0,finish=1;
	memset(vis,false,sizeof(vis));
	fortodo(i,1,T) d[i]=inf;
	Queue[1]=S;vis[S]=true;d[S]=0;
	while (front<finish){
		int x=Queue[++front];
		for (int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[t[i]]>d[x]+cost[i])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=true;
				}
			}
		vis[x]=false;
	}
	return d[T]!=inf;
}

inline void Augment(){
	int aug=inf;
	for (int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for (int i=T;i!=S;i=pre[i]){ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

int main(){
	judge();
	buildmap();
	fortodo(i,1,T) beifenhead[i]=head[i];
	fortodo(i,0,num){
		beifenne[i]=ne[i];
		beifensap[i]=sap[i];
		beifencost[i]=cost[i];
		beifent[i]=t[i];
	}
	while (SPFA()) Augment();
	cout<<ans<<endl;ans=0;
	fortodo(i,1,T) head[i]=beifenhead[i];
	fortodo(i,0,num){
		ne[i]=beifenne[i];
		sap[i]=beifensap[i];
		cost[i]=-beifencost[i];
		t[i]=beifent[i];
	}
	while (SPFA()) Augment();
	cout<<-ans;
	return 0;
}
```
---
# #15
#### 题目描述
> G 公司有n 个沿铁路运输线环形排列的仓库，每个仓库存储的货物数量不等。如何用最少搬运量可以使n 个仓库的库存数量相同。搬运货物时，只能在相邻的仓库之间搬运。

#### 题解
傻逼费用流

``` c++

#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=205,maxe=20005,inf=1<<29;
int head[maxn],ne[maxe],sap[maxe],cost[maxe],t[maxe],edge[maxn],pre[maxn],d[maxn],S,T,tot,ans=0,num,n,Queue[maxe];
int vis[maxn];

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}

inline int dis(int i,int j){
	if (i>j) swap(i,j);
	return min((j-i),(i+n-j));
}
inline void init(){
	int x;
	scanf("%d",&n);S=0;T=2*n+1;
	fortodo(i,S,T) head[i]=-1;
	fortodo(i,1,n){scanf("%d",&x);addedge(S,2*i-1,x,0);tot+=x;addedge(2*i-1,2*i,inf,0);}
	tot/=n;
	fortodo(i,1,n) fortodo(j,1,n){
		if (i==j) continue;
		addedge(2*i-1,2*j,inf,dis(i,j));
	}
	fortodo(i,1,n) addedge(2*i,T,tot,0);
}

inline bool SPFA(){
	memset(vis,0,sizeof(vis));
	int front=0,finish=1;
	fortodo(i,S,T) d[i]=inf;
	Queue[1]=S;d[S]=0;vis[S]=true;
	while(front<finish){
		int x=Queue[++front];
		for (int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[t[i]]>d[x]+cost[i])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=true;
				}
			}
		vis[x]=false;
	}
	return d[T]!=inf;
}

inline void Augment(){
	int aug=inf;
	for(int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for(int i=T;i!=S;i=pre[i]){ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}

int main(){
	judge();
	init();
	while (SPFA()) Augment();
	cout<<ans;
	return 0;
}
```
---
# #16
#### 题目描述
>深海资源考察探险队的潜艇将到达深海的海底进行科学考察。潜艇内有多个深海机器
人。潜艇到达深海海底后，深海机器人将离开潜艇向预定目标移动。深海机器人在移动中还
必须沿途采集海底生物标本。沿途生物标本由最先遇到它的深海机器人完成采集。每条预定
路径上的生物标本的价值是已知的，而且生物标本只能被采集一次。本题限定深海机器人只
能从其出发位置沿着向北或向东的方向移动，而且多个深海机器人可以在同一时间占据同一
位置。

#### 题解

这个问题可以看做是多出发点和目的地的网络运输问题。每条边的价值只能计算一次，容量限制要设为1。同时还将要连接上容量不限，费用为0的重边。由于“多个深海机器人可以在同一时间占据同一位置”，所以不需限制点的流量，直接求费用流即可。

``` c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxm=21,maxn=3001,maxe=200001,inf=1<<30;
int P,Q,a,b,n,m,head[maxn],ne[maxe],sap[maxe],cost[maxe],t[maxe],Queue[maxe],d[maxn],pre[maxn],edge[maxn],num=0,S,T,ans=0;
bool vis[maxn];

inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}
inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline int place(int x,int y){
	return y*(Q+1)+x+1;
}
inline void init(){
	scanf("%d%d",&a,&b);scanf("%d%d",&P,&Q);S=0;T=place(Q,P)+1;int k,x,y;
	fortodo(i,S,T) head[i]=-1;
	fortodo(i,0,P) 
		fortodo(j,0,Q-1){
			scanf("%d",&x);
			addedge(place(j,i),place(j+1,i),1,x);
			addedge(place(j,i),place(j+1,i),inf,0);
		}
	fortodo(i,0,Q) fortodo(j,0,P-1){
		scanf("%d",&x);
		addedge(place(i,j),place(i,j+1),1,x);
		addedge(place(i,j),place(i,j+1),inf,0);
	}
	fortodo(i,1,a){
		scanf("%d%d%d",&k,&x,&y);
		addedge(S,place(y,x),k,0);
	}
	fortodo(i,1,b){
		scanf("%d%d%d",&k,&x,&y);
		addedge(place(y,x),T,k,0);
	}
}

inline bool SPFA(){
	memset(vis,false,sizeof(vis));
	int front=0,finish=1;
	fortodo(i,S,T) d[i]=-inf;
	Queue[1]=S;vis[S]=1;d[S]=0;
	while(front<finish){
		int x=Queue[++front];
		for(int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[x]+cost[i]>d[t[i]])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=true;
				}
			}
		vis[x]=false;
	}
	return d[T]!=-inf;
}

inline void Augment(){
	int aug=inf;
	for(int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for(int i=T;i!=S;i=pre[i]){ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}

int main(){
	judge();
	init();
	while (SPFA()) Augment();
	cout<<ans;
	return 0;
}
```
---
# #17
#### 题目描述
给定实直线L 上n 个开区间组成的集合I，和一个正整数k，试设计一个算法，从开区间集合I 中选取出开区间集合S，使得在实直线L 的任何一点x，S 中包含点x 的开区间个数不超过k，且总长度达到最大。这样的集合S称为开区间集合 I的最长 k可重区间集。| z |称为最长 k可重区间集的长度。
#### 题解
按左端点排序所有区间，把每个区间拆分看做两个顶点（i.a）（i.b），建立附加源S汇T，以及附加顶点S'。

1、连接S到S'一条容量为K，费用为0的有向边。
2、从S'到每个（i.a）连接一条容量为1，费用为0的有向边。
3、从每个（i.b）到T连接一条容量为1，费用为0的有向边。
4、从每个顶点（i.a）到（i.b）连接一条容量为1，费用为区间长度的有向边。
5、对于每个区间i，与它右边的不相交的所有区间j各连一条容量为1，费用为0的有向边。

求最大费用最大流，最大费用流值就是最长k可重区间集的长度。
```c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=1005,maxe=10005,inf=1<<29;
int head[maxn],ne[maxe],sap[maxe],t[maxe],cost[maxe],S,SS,T,num,n,k,d[maxn],Queue[maxe],pre[maxn],edge[maxn],ans;
bool vis[maxn];

struct seg{
	int x,y;
}inter[maxn];

inline bool cmp(const seg &a,const seg &b){
	return (a.x<b.x);
}

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}

inline void init(){
	scanf("%d%d",&n,&k);
	fortodo(i,1,n) scanf("%d%d",&inter[i].x,&inter[i].y);
}
inline void buildmap(){
	sort(inter+1,inter+n,cmp);
	S=1;SS=2*n+2;T=SS+1;fortodo(i,S,T) head[i]=-1;addedge(S,SS,k,0);
	fortodo(i,1,n){
		addedge(SS,2*i,1,0);
		addedge(2*i,2*i+1,1,inter[i].y-inter[i].x);
		addedge(2*i+1,T,1,0);
		fortodo(j,i+1,n) if (inter[j].x>=inter[i].y) addedge(2*i+1,2*j,1,0);
	}
}

inline bool SPFA(){
	memset(vis,0,sizeof(vis));
	fortodo(i,S,T) d[i]=-inf;
	int front=0,finish=1;
	Queue[1]=S;d[S]=0;vis[S]=1;
	while(front<finish){
		int x=Queue[++front];
		for(int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[t[i]]<d[x]+cost[i])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=1;
				}
			}
		vis[x]=0;
	}
	return d[T]!=-inf;
}

inline void Augment(){
	int aug=inf;
	for(int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for(int i=T;i!=S;i=pre[i]){ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}
int main(){
	judge();
	init();
	buildmap();
	while (SPFA()) Augment();
	cout<<ans;
	return 0;
}
```
---
# #18
#### 题目描述
>给定二维平面 上n 个开线段组成的集合I，和一个正整数k，试设计一个算法，从开线段集合I 中选取出开线段集合S，使得任何一条线段x=t，S 中与该线段相交的开线段个数不超过k，且总长度达到最大。这样的集合S称为开线段集合 I的最长 k可重区间集。| z |称为最长 k可重线段集的长度。

#### 题解
同上。。。
```c++
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#define sqr(a) (a*a)
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxn=3001,maxe=100001,inf=1<<29;
int ans,head[maxn],ne[maxe],sap[maxe],cost[maxe],t[maxe],pre[maxn],edge[maxn],Queue[maxe],d[maxn],S,SS,T,num,n,k;
bool vis[maxn];
struct seg{
	int x,y,length;
}inter[maxn];
int cmp(const seg &a,const seg &b){return a.x<b.x;}
inline void addedge(int begin,int end,int flow,int value){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	cost[num]=value;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	cost[num]=-value;
	t[num++]=begin;
}
inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void init(){
	scanf("%d%d",&n,&k);int x1,y1,x2,y2;
	fortodo(i,1,n){
		scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
		inter[i].x=x1;inter[i].y=x2;
		inter[i].length=floor((long double)sqrt((x1-x2)*(x1-x2)+(long double)(y1-y2)*(y1-y2)));
	}
}

inline void buildmap(){
	sort(inter+1,inter+1+n,cmp);
	S=1;SS=2*n+2;T=SS+1;fortodo(i,S,T) head[i]=-1;addedge(S,SS,k,0);
	fortodo(i,1,n){
		addedge(SS,2*i,1,0);
		addedge(2*i,2*i+1,1,inter[i].length);
		addedge(2*i+1,T,1,0);
		fortodo(j,i+1,n) if (inter[j].x>=inter[i].y) addedge(2*i+1,2*j,1,0);
	}
}

inline bool SPFA(){
	memset(vis,0,sizeof(vis));
	fortodo(i,S,T) d[i]=-inf;
	int front=0,finish=1;
	Queue[1]=S;d[S]=0;vis[S]=1;
	while(front<finish){
		int x=Queue[++front];
		for(int i=head[x];i!=-1;i=ne[i])
			if ((sap[i])&&(d[x]+cost[i]>d[t[i]])){
				d[t[i]]=d[x]+cost[i];
				pre[t[i]]=x;
				edge[t[i]]=i;
				if (!vis[t[i]]){
					Queue[++finish]=t[i];
					vis[t[i]]=1;
				}
			}
		vis[x]=0;
	}
	return d[T]!=-inf;
}

inline void Augment(){
	int aug=inf;
	for(int i=T;i!=S;i=pre[i]) aug=min(aug,sap[edge[i]]);
	for(int i=T;i!=S;i=pre[i]){ans+=aug*cost[edge[i]];sap[edge[i]]-=aug;sap[edge[i]^1]+=aug;}
}
int main(){
	judge();
	init();
	buildmap();
	while (SPFA()) Augment();
	cout<<ans;
	return 0;
}
```
---
# #19
#### 题目描述
> 在一个n*n个方格的国际象棋棋盘上，马（骑士）可以攻击的棋盘方格如图所示。棋盘上某些方格设置了障碍，骑士不得进入。对于给定的n*n个方格的国际象棋棋盘和障碍标志，计算棋盘上最多可以放置多少个骑
士，使得它们彼此互不攻击。


#### 题解
同#8


```c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#define fortodo(i,a,b) for (int i=(a);i<=(b);i++)
using namespace std;
const int maxm=205,maxn=40005,maxe=400005,inf=1<<29;
int d[maxn],cur[maxn],head[maxn],ne[maxe],sap[maxe],t[maxe],pre[maxn],edge[maxn],his[maxn],S,T,n,m,num,countnum[maxn],ans=0;
const int dx[8]={2,1,-1,-2,-2,-1,1,2},dy[8]={1,2,2,1,-1,-2,-2,-1};
bool vis[maxm][maxm];

inline void judge(){
	freopen("in.txt","r",stdin);
	freopen("out.txt","w",stdout);
}

inline void addedge(int begin,int end,int flow){
	ne[num]=head[begin];
	head[begin]=num;
	sap[num]=flow;
	t[num++]=end;
	ne[num]=head[end];
	head[end]=num;
	sap[num]=0;
	t[num++]=begin;
}

inline int place(int x,int y){
	return (n*(x-1)+y);
}

inline void init(){
	memset(vis,1,sizeof(vis));
	scanf("%d%d",&n,&m);S=0;T=place(n,n)+1;int x,y;
	fortodo(i,1,m){scanf("%d%d",&x,&y);vis[x][y]=false;}
	fortodo(i,S,T) head[i]=-1;
}

inline void buildmap(){
	int x,y;
	fortodo(i,1,n) fortodo(j,1,n){
		if (!vis[i][j]) continue;
		if (((i+j)&1)==0)
			addedge(place(i,j),T,1);
		else{
			addedge(S,place(i,j),1);
			fortodo(k,0,7){
				x=i+dx[k];y=j+dy[k];
				if (((x>=1)&&(x<=n)&&(y>=1)&&(y<=n))) addedge(place(i,j),place(x,y),inf);
			}
		}
	}
}

inline int isap(){
	fortodo(i,S,T) cur[i]=head[i];
	countnum[0]=T+1;
	int i=S,aug=inf,jl,minnum;
	while (d[S]<T+1){
		his[i]=aug;
		bool flag=false;
		for (int j=cur[i];j!=-1;j=ne[j])
			if ((sap[j])&&(d[t[j]]+1==d[i])){
				pre[t[j]]=i;
				edge[i]=j;
				aug=min(aug,sap[j]);
				cur[i]=j;
				flag=true;
				i=t[j];
				if (i==T){
					ans+=aug;
					while (i!=S){
						i=pre[i];
						sap[edge[i]]-=aug;
						sap[edge[i]^1]+=aug;
					}
					aug=inf;
				}
				break;
			}
		if (flag) continue;
		minnum=T;
		for(int j=head[i];j!=-1;j=ne[j]) if ((sap[j])&&(d[t[j]]<minnum)){minnum=d[t[j]];jl=j;}
		countnum[d[i]]--;
		if (countnum[d[i]]==0) return ans;
		d[i]=minnum+1;
		cur[i]=jl;
		countnum[d[i]]++;
		if(i!=S){i=pre[i];aug=his[i];}
	}
	return ans;
}
int main(){
	judge();
	init();
	buildmap();
	cout<<n*n-m-isap();
	return 0;
}
```