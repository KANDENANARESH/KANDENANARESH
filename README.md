#include<stdio.h>
#include<stdlib.h>
struct node
{
	int data;
	struct node* next;
};
struct Graph
{
	int numver;
	struct node** adjl;
	int* visited;
};
struct node* createnode(int data)
{
	struct node* new=(struct node*)malloc(sizeof(struct node));
	new->data=data;
	new->next=NULL;
	return new;
}
struct Graph* createGraph(int n)
{
	struct Graph* graph=(struct Graph*)malloc(sizeof(struct Graph));
	graph->numver=n;
	graph->adjl=(struct node*)malloc(n*sizeof(struct node));
	graph->visited=(int*)malloc(n*sizeof(int));
	for(int i=0;i<n;i++)
	{
		graph->adjl[i]=NULL;
		graph->visited[i]=0;
	}
	return graph;
}
void addedge(struct Graph* graph,int s,int d)
{
	struct node* new=createnode(d);
	new->next=graph->adjl[s];
	graph->adjl[s]=new;
	
	 new=createnode(s);
	new->next=graph->adjl[d];
	graph->adjl[d]=new;
}
void bfs(struct Graph* graph,int stratver)
{
	int* que=(int*)malloc(graph->numver * sizeof(int));
	int f=0;
	int r=0;
	graph->visited[stratver]=1;
	que[r++]=stratver;
	while(f<r)
	{
		int current=que[f++];
		printf("%d ",current);
		struct node* temp=graph->adjl[current];
		while(temp!=NULL)
		{
			int adjv=temp->data;
			if(graph->visited[adjv]==0)
			{
				graph->visited[adjv]=1;
				que[r++]=adjv;
			}
			temp=temp->next;
		}
	}
	free(que);
}
void dfs(struct Graph* graph,int stratver)
{
	int *stack=(int*)malloc(graph->numver*sizeof(int));
	int top=-1;
	stack[++top]=stratver;
	while(top>=0)
	{
		int current=stack[top--];
		if(graph->visited[current]==0)
		{
			graph->visited[current]=1;
			printf("%d ",current);
			struct node* temp=graph->adjl[current];
			while(temp!=NULL)
			{
			int adjv=temp->data;
			if(graph->visited[adjv]==0)
			{
				stack[++top]=adjv;
			}
			temp=temp->next;
		}
		}
	}
	free(stack);
}	
int main()
{
	int a,b,stratver,n;
	printf("enter the no' of vertices:");
	scanf("%d",&n);
	struct Graph* graph=createGraph(n);
	for(int i=0;i<n;i++)
	{
		printf("enter the edges of vertices:");
		scanf("%d%d",&a,&b);
		addedge(graph,a,b);
	}
	printf("enter the strating verix:");
	scanf("%d",&stratver);
   printf("BFS TRAVERSAL:");
   bfs(graph, stratver);
	printf("\n");
	
	printf("DFS TRAVERSAL:");
	dfs(graph, stratver);
	printf("\n");
	return 0;
}
