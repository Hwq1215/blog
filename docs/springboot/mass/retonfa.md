# 正规式到NFA
## 数据结构设计

#### Node节点类：
```c++
class Node{
    public:
    int name;
    
    public:
    Node(){
        this->name = -1;
    }

    Node(int name){ 
        this->name = name;
    }

    inline bool operator==(const Node nodeB){
        return this->name==nodeB.name;
    }

    inline bool operator>(const Node nodeB){
        return this->name>nodeB.name;
    }

    inline bool operator<(const Node nodeB){
        return this->name<nodeB.name;
    }

    inline int operator-(const Node nodeB){
        return this->name - nodeB.name;
    }

    int add(){
        this->name += 1;
        return this->name;
    }

    int sub(){
        this->name -= 1;
        return this->name;
    }

    int sub(int n){
        this->name -= n;
        return this->name;
    }

};
```

#### Edge边类
```cpp
   class Edge{
    public:
    Node in_node;
    Node out_node;
    char node_charater;
        public:
    Edge(){
        this->in_node = Node();
        this->out_node = Node();
        char node_charater = null_symbol;
    };
    Edge(Node in_node,Node out_node,char node_character){
        this->in_node = in_node;
        this->out_node = out_node;
        this->node_charater = node_character;
    }
   } 
```
#### CellEdge边的集合，可以理解为图
```cpp
class CellEdge{
    private:
    Node start_node;        //开始节点
    Node end_node;          //结束节点
    vector<Edge> edges;     //边的集合
    int edges_num;          //边的个数
    int nodes_num;          //节点个数

    public:
    //初始化
    CellEdge(){
        this->start_node = Node();
        this->end_node = Node();
        this->edges.resize(0);
        this->edges_num = 0;
        this->nodes_num = 0;
    };

    CellEdge(int &nodeNum,char _character_){
        this->edges_num = 0;
        this->nodes_num = 0;
        this->edges.resize(0);
        nodeNum ++;
        Node inNode = getNode(nodeNum);
        nodeNum ++;
        Node outNode = getNode(nodeNum);
        Edge edge(inNode,outNode,_character_);

        this->start_node = inNode;
        this->end_node = outNode;
        this->edges.push_back(edge);
        this->edges_num++;
        this->nodes_num += 2;    //获得两个新节点
    }

    int edgeSize(){
        return this->edges_num;
    }

    int nodeSize(){
        return this->nodes_num;
    }

    vector<Edge> getEdges(){
        return this->edges;
    }

    //重新调整
    void reRange(){
        int i = minNumOfNode();
        for(int i = minNumOfNode();i<maxNumOfNode();i++){
            int j = 0;
            for(j = 0;j<this->edges.size();j++){
                if(this->edges[j].in_node == i || this->edges[j].out_node == i ) break;
            }
            if(j == this->edges.size()){
                for(auto &e: this->edges){
                    e.in_node.name > i ? e.in_node.sub():0;
                    e.out_node.name > i? e.out_node.sub():0;
                }
                if(this->end_node > i) this->end_node.sub();
                i--;
            }
        }
    }

    //与运算 a&b
    CellEdge Join(CellEdge b){
        vector<Edge> insEdges;
        for(Edge edge: b.getEdges()){
            if(edge.in_node == b.start_node){
                edge.in_node = this->end_node;
            }
            insEdges.push_back(edge);
        }
        this->edges.insert(this->edges.end(),insEdges.begin(),insEdges.end());
        this->end_node = b.end_node;
        this->edges_num += b.edges_num;
        this->nodes_num += b.nodes_num-1; //与运算获b的新节点
        this->reRange();
        return *this;
    }

    //或运算 a|b
    CellEdge Select(int &nodeNum,CellEdge b){
        //拿到两个新节点
        nodeNum ++;
        Node nStartNode = getNode(nodeNum);
        nodeNum ++;
        Node nEndNode = getNode(nodeNum);

        vector<Edge> edges1{Edge(nStartNode,this->start_node,null_symbol),
                            Edge(nStartNode,b.start_node,null_symbol),
                            Edge(this->end_node,nEndNode,null_symbol),
                            Edge(b.end_node,nEndNode,null_symbol),
        };
        this->edges.insert(this->edges.end(),edges1.begin(),edges1.end());
        this->edges_num += 4;
        this->edges.insert(this->edges.end(),b.edges.begin(),b.edges.end());
        this->edges_num += b.edges_num;
        this->start_node = nStartNode;
        this->end_node = nEndNode;

        this->nodes_num += b.nodes_num+2;
        return *this;
    }

    //闭包运算 a*
    CellEdge Cycle(int &nodeNum){
        nodeNum ++;
        Node nStartNode = getNode(nodeNum);
        nodeNum ++;
        Node nEndNode = getNode(nodeNum);
        vector<Edge> edges1{Edge(nStartNode,this->start_node,null_symbol),
                            Edge(nStartNode,nEndNode,null_symbol),
                            Edge(this->end_node,this->start_node,null_symbol),
                            Edge(this->end_node,nEndNode,null_symbol),
        };
        this->edges.insert(this->edges.end(),edges1.begin(),edges1.end());
        this->edges_num += 4;
        this->nodes_num += 2;       //增加2个新节点
        this->start_node = nStartNode;
        this->end_node = nEndNode;
        return *this;
    }

    Node getNode(int NodeNum){
        return Node(NodeNum);
    }

    int getStartNodeName(){
        return this->start_node.name;
    }

    int getEndNodeName(){
        return this->end_node.name;
    }
 
    int maxNumOfNode(){
        int max = -1;
        for(auto edge:this->edges){
            int m = edge.in_node.name>edge.out_node.name ? edge.in_node.name:edge.out_node.name;
            max = max<m? m:max;
        }
        return max;
    }

    int minNumOfNode(){
        int minN = INT32_MAX;
        for(auto edge:this->edges){
            int m = edge.in_node.name<edge.out_node.name ? edge.in_node.name:edge.out_node.name;
            minN = minN>m? m:minN;
        }
        return minN;
    }
};
```