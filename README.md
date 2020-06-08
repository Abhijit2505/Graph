# Graph
This repository contains tricks and techniques involved with the Graph data structures.
## Algorithms and Designs

### 1. Adjacent Matrix Creation and Edge List Marking

Creating an adjacent matrix is necessary. Suppose we have a graph like below,
![graph image](https://github.com/Treasure-Code-Algorithm/Graph/blob/master/graph.PNG)

The adjacent matrix can be represented as,
![adjacent matrix](https://github.com/Treasure-Code-Algorithm/Graph/blob/master/adjacent%20matrix.PNG)

### 2. Depth First Search (DFS) - Graph Traversal Technique

<b>PseudoCode:</b>

    print(strating_vertex)
    visited[starting_vertex] = true
    for(i to num_edges)
        if(i == staring_vertex)
            DO NOTHING, CONTINUE 
        if edge beteween starting_vertex & i:
              if(i is visited): 
                  DO NOTHING, CONTINUE    
               recursive call <- starting_vertex will be i
               
<b>Original Code in C++:</b>

    void printDFS(int** edges_list,int num_nodes,int starting_vertex,bool* visited_nodes)
    {
        cout << starting_vertex << endl;
        visited_nodes[starting_vertex] = true;
        for(int i=0;i<num_nodes;i++)
        {
            if(i==starting_vertex)
            {
                continue;
            }
            if(edges_list[starting_vertex][i]==1)
            {
                if(visited_nodes[i])
                {
                    continue;
                }
                printDFS(edges_list,num_nodes,i,visited_nodes);
            }
        }
    }

### 2. Bredth First Search (BFS) - Graph Traversal Technique

<b>PseudoCode:</b>

    build a queue
    add starting_vertex to the queue
    while(queue is not empty)
    {
       front_node = queue.front()
       pop the front node
       print(front_node)
       for(i=0 to num_edges)
       {
            if(i==front_node)
            {
                DO NOTHING, CONTINUE
            }
            if(edge between front_node and i)
            {
                if(visited_node[i]==1) 
                {
                    DO NOTHING, CONTINUE
                }
                push i to ther queue
                visited_node[i] = true
            }
        }
    }
    
<b>Original Code in C++:</b>    
    
    void printBFS(int** edges_list,int num_nodes,int starting_vertex,bool* visited_nodes)
    {
        queue<int> pending_nodes;
        pending_nodes.push(starting_vertex);
        while(!pending_nodes.empty())
        {
            int front_node = pending_nodes.front();
            cout << front_node << " ";
            visited_nodes[front_node]=true;
            pending_nodes.pop();
            for(int i=0;i<num_nodes;i++)
            {
                if(i==front_node)
                {
                    continue;
                }
                if(edges_list[front_node][i]==1)
                {
                    if(visited_nodes[i])
                    {
                        continue;
                    }
                    pending_nodes.push(i);
                    visited_nodes[i]=true;
                }
            }
        }
    }
### 3. Has Path - Checking if there is a path from one node to the other

<b>PseudoCode:</b>

    is there a direct path ? true:false
    for(i=0 to num_nodes)
    {
        if(edge between starting vertex and i)
        {
            if(not visited[i])
            {
                bool ans = recursive call:staring vertex = i this time
                if(ans) return true
            }
        }
    }
    
<b>Original Code in C++:</b>    
    
    bool has_path_helper(int** edges_list,int num_nodes,int starting_vertex,int ending_vertex,bool* visited)
    {
        if(edges_list[starting_vertex][ending_vertex]==1)
        {
            return true;
        }
        visited[starting_vertex] = true;
        for(int i=0;i<num_nodes;i++)
        {
            if(i==starting_vertex)
            {
                continue;
            }
            if(edges_list[starting_vertex][i]==1)
            {
                if(!visited[i])
                {
                    visited[i] = true;
                    bool ans = has_path_helper(edges_list,num_nodes,i,ending_vertex,visited);
                    if(ans)
                    {
                        return true;
                    }
                } 
            }
        }
        return false;
    }

    bool has_path(int** edges_list,int num_nodes,int starting_vertex,int ending_vertex)
    {
        bool* visited = new bool[num_nodes];
        for(int i=0;i<num_nodes;i++)
        {
            visited[i] = false;
        }
        return has_path_helper(edges_list,num_nodes,starting_vertex,ending_vertex,visited);
    }

### 3. Has Path - Checking if there is a path from one node to the other

<b>PseudoCode:</b>

    if(starting_vertex==ending_vertex)
    {
        vector.push_back(ending_vertex)
        return vector
    }
    visited[starting_vertex] = true
    for(i = 0 to num_nodes)
    {
        if(i equals starting_vertex)
        {
            DO NOTHING, CONTINUE
        }
        if(edge between starting_vertex and i)
        {
            if(not visited[i])
            {
                small_ans = recrsive call: starting_vertex = i
                if(small_ans is not NULL)
                {
                    small_ans add starting_vertex
                    return small_ans
                }
            }
        }
    }
    return NULL

<b>Original Code in C++:</b>    

    vector<int>* pathDFS(int** edges_list,int num_nodes,int starting_vertex,int ending_vertex, bool* visited)
    {
        if(starting_vertex == ending_vertex)
        {
            vector<int>* path_nodes = new vector<int>();
            path_nodes -> push_back(ending_vertex);
            return path_nodes;
        }
        visited[starting_vertex] = true;
        for(int i=0;i<num_nodes;i++)
        {
            if(i==starting_vertex)
            {
                continue;
            }
            if(edges_list[starting_vertex][i]==1)
            {
                if(!visited[i])
                {
                    vector<int>* small_ans = pathDFS(edges_list,num_nodes,i,ending_vertex,visited);
                    if(small_ans != NULL)
                    {
                        small_ans -> push_back(starting_vertex);
                        return small_ans;
                    }
                }
            }
        }
        return NULL;
    }
