---
tags:
  - 网络流
  - 图论
---
# 网络流

用水管来类比：

> 自来水厂（源点）要经过水管（边）把水运到石老板家（汇点）。
> 每条单向水管连接两个站点，水管都有一个最大流量，源点有无限的水。显然，每条水管可能有水流，可能没有。
> 问：自来水厂 **最多** 能给石老板送多少水？

## 网络与流

### 定义

+ **网络**：图（一般为有向无重边）
+ **容量网络**：边权为该边容量的网络
+ **流量网络**：边权为该边实际流量的网络
+ **剩余网络**：容量网络边权减去剩余网络对应边权形成的网络
+ **满流**：流量网络的流量达到最大（即**流入汇点的流量最大** ）
+ **增广路**：剩余网络中从源到汇使路径中最小剩余边权不为零的路径
+ **源 & 汇**：所有流量从源流出，流入汇点

### 性质

** 剩余网络在满流时不会有一条增广路**

若满流时剩余网络仍有增广路，则沿着增广路增加流量总能使最终流量增加

** 任何一个合法的流量网络中，除了源和汇外的所有节点，流入量之和等于流出量之和**

直观理解：水不会在中间节点凭空产生或消失

## 割

### 定义

给定一张图 $G=\{V,E\}$ 和源点 $A$，汇点 $B$。

割集 $C(S,T)$ 被定义为 $\{(u,v) |u\in S,v\in T,(u,v)\in E\}$

即 $C(S,T)$ 为给定 $S,T$ 两点集间的边集

**最小割**：**边权和最小**的割集 $C(S,T)$，使 $A\in S,B\in T$。

即花费最小代价把源点到汇点的所有路径切断，使源点汇点不连通，切断一条边的代价为它的边权。

### 性质

** 一个流量网络的每一个割集，对应的流量和都等于总流量 **

一个形象的例子：恐怖分子切断了若干水管，使原来流向石老板家的水全部流走，最终从被切断水管流出的水量就是原来流向石老板家的水

** 同一张图上，每一个割都大于等于每一个流 **

考虑任意一个割集和任意一个流，由上一条性质得，割集对应的流量等于总流量，而割集对应的边权和大于等于边权对应流量（每一条边的流量小于等于边权）

** 最大流等于最小割 **

满流时，根据增广路的性质，剩余网络中源点汇点不存在最小剩余边权不为零的路径。**断开所有剩余流量为零的边**，另 $S$ 为所有与源点连通的点集，$T=V-S$，则割集 $C(S,T)$ 包含的所有边**剩余流量**为0，即**流量等于容量**。由此得 $C(S,T)$ 的边权和为最大流的流量。由上一条性质得，$C(S,T)$ 为最小割。综上，最大流等于最小割。

## 求最大流

部分引自 [OI-Wiki](https://oi-wiki.org/graph/flow/max-flow/)

这里介绍以下算法：

+ Edmonds-Karp
+ ISAP
+ 预流推进 & HLPP

???+ tip "小贴士"
    考场上，尽量使用比较好写的 `Dinic` 或 `ISAP`，这两个算法虽然比 `HLPP` 理论复杂度高，但足以解决绝大多数问题，而且好写。

### Edmonds-Karp

由于最大流的性质，我们可以每次寻找增广路

### ISAP

### 预流推进 & HLPP

前面的算法都基于寻找增广路来求解最大流，这里再讲一些不基于增广路的算法。

预流推进的核心在于允许节点的流入大于流出。我们把允许流入大于流出的网络称为预流。

预流推进算法维护每个结点的高度 $h(u)$ ，并且规定溢出的结点 $u$ 如果要推送超额流，只能向高度小于 $u$ 的结点推送；如果 $u$ 没有相邻的高度小于 $u$ 的结点，就增加 $u$ 的高度（重贴标签）。

初始化时，$h(s)=n,h(other)=0$，并沿边推送起始节点流量。

最高标号预流推进算法 ( `HLPP` ) 与预流推进的区别在于，在预流推进时，`HLPP` 会优先推送高度大的节点的流量。为了在随机图上跑出接近 `Dinic` 速度，我们还可以对 `HLPP` 进行如下优化：

+ 将 $h(u)$ 初始化为 $dis(u,t)$，将 $h(s)$ 仍初始化为 $n$。用 BFS 求出 $dis(u,t)$，即可减少大量重贴标签操作。
+ 用桶代替优先队列，复杂度降低一个 $\log$。
+ 因为每次取出的是最高节点，而重贴标签又只会增加当前节点高度，所以如果当前节点高度上没有其他节点，比当前节点高的所有节点都不可能向当前节点以后的节点推送流量，所以为了尽快将流量推送回 $s$，我们把所有比当前节点高的节点高度变为 $n+1$。

优化后时间复杂度为 $O(n^2\sqrt m)$

## 例题

[P3376 【模板】网络最大流](https://www.luogu.com.cn/problem/P3376)

网络流模板。

??? note "[P3376 【模板】网络最大流](https://www.luogu.com.cn/problem/P3376) 参考代码"
    留坑

[P4722 【模板】最大流 加强版 / 预流推进](https://www.luogu.com.cn/problem/P4722)

只有用 `HLPP` 才能过。

??? note "[P4722 【模板】最大流 加强版 / 预流推进](https://www.luogu.com.cn/problem/P4722) 参考代码（使用 HLPP）"
    ![AC 100Pts](https://s3.bmp.ovh/imgs/2021/11/01ac9d5e6df5a941.jpg)

    部分参考 OI-Wiki 与 P4722 题解，因为本人巨喜欢空行，核心部分写了 65 行，一共 110 行。

    ```cpp
    #include`<iostream>`
    #include `<cstdio>`
    #include `<cstring>`
    #include `<algorithm>`
    #include `<vector>`
    #include `<queue>`
    // stO OI-Wiki Orz
    #define il inline
    #define For(i,l,u) for(int i=(l);i<=(u);i++)
    #define RFor(i,l,u) for(int i=(u);i>=(l);i--)
    const int INF=2147483647;

    using namespace std;

    const int maxn=2405,maxm=120005;

    int n,m,s,t;

    namespace E{
    	int f[maxm*2],t[maxm*2],v[maxm*2],nxt[maxm*2],oth[maxm*2];
    	int tot,fst[maxn];
    	il int add(int u_,int v_,int w_){	// add edge
    		++tot;f[tot]=u_,t[tot]=v_,v[tot]=w_,nxt[tot]=fst[u_],    fst[u_]=tot;return tot;
    	}
    	il void addf(int u,int v,int w){	// add edge and     inverse edge
    		int d1=add(u,v,w),d2=add(v,u,0);
    		oth[d1]=d2;oth[d2]=d1;
    	}
    }

    #define ForP(u,i) for(int i=E::fst[u];i;i=E::nxt[i])

    namespace Flow{

    int h[maxn],e[maxn],vis[maxn],gap[maxn],mxh;
    	vector`<int>` mp[maxn];

    il void bfs(int s){
    		static queue`<int>` q;
    		while(!q.empty()){ q.pop(); }
    		q.push(s);h[s]=0;vis[s]=1;
    		while(!q.empty()){
    			int u=q.front(),v;q.pop();
    			ForP(u,i){
    				if(!vis[v=E::t[i]] && E::v[E::oth[i]]){
    					vis[v]=1;h[v]=h[u]+1;q.push(v);
    				}
    			}
    		}
    	}
    	il int push(int u){
    		bool init=u==s;	// good idea from OI-Wiki
    		ForP(u,i){
    			int v=E::t[i],w=E::v[i];
    			if(w==0 || ( !init && h[v]!=h[u]-1) ){ continue; }
    			int sw=init?w:min(w,e[u]);	// from OI-Wiki too
    			if(e[v]==0 && v!=s && v!=t){ mp[h[v]].push_back(v);    mxh=max(mxh,h[v]); }
    			e[u]-=sw;e[v]+=sw;E::v[i]-=sw;E::v[E::oth[i]]+=sw;
    			if(e[u]==0){ return 0; }
    		}
    		return e[u];
    	}
    	il void relabel(int u){
    		h[u]=INF;
    		ForP(u,i){ if(E::v[i]){ h[u]=min(h[u],h[E::t[i]]); } }
    		if(++h[u]<n){
    			mp[h[u]].push_back(u);mxh=max(mxh,h[u]);gap[h[u]]    ++;
    		}
    	}
    	il int sel(){
    		while(mp[mxh].size()==0 && mxh>=0){ mxh--; }
    		return mxh==-1?0:mp[mxh][mp[mxh].size()-1];
    	}

    il int HLPP(){
    		For(i,1,n){ h[i]=INF; } bfs(t);
    		if(h[s]==INF){ return 0; }
    		For(i,1,n){ if(h[i]!=INF){gap[h[i]]++;} }
    		h[s]=n;push(s);
    		int u;
    		while(u=sel()){		// from OI-Wiki
    			mp[mxh].pop_back();
    			if(push(u)){
    				if((--gap[h[u]])==0){
    					For(i,1,n){
    						if(i!=s && i!=t && h[u]<h[i] && h[i]<n    +1){
    							h[i]=n+1;
    						}
    					}
    				}
    				relabel(u);
    			}
    		}
    		return e[t];
    	}

    }

    int main(){

    scanf("%d%d%d%d",&n,&m,&s,&t);
    	for(int i=1;i<=m;i++){
    		int u,v,w;scanf("%d%d%d",&u,&v,&w);
    		E::addf(u,v,w);
    	}

    printf("%d\n",Flow::HLPP());

    return 0;
    }

    ```

很多问题都可以转化为网络流问题，建议参考 [网络流24题](https://www.luogu.com.cn/training/42162)
