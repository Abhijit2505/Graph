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

### 4. Print Path - Printing the path between two given nodes, Using DFS

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
    
### 5. Print Path - Printing the path between two given nodes, Using BFS

<b>Main Code in C++</b>

    vector<int>* pathBFS(int** edges,int num_nodes,int starting_vertex,int ending_vertex)
    {
         queue<int> shortest_path;
         bool* visited = new bool[num_nodes];
         for(int i=0;i<num_nodes;i++)
         {
             visited[i] = false;
         }
         shortest_path.push(starting_vertex);
         visited[starting_vertex] = true;
         bool done = false;
         unordered_map<int,int> parent;
         while(!shortest_path.empty() && !done)
         {
             int front_node = shortest_path.front();
             shortest_path.pop();
             for(int i=0;i<num_nodes;i++)
             {
                 if(edges[front_node][i] && !visited[i])
                 {
                     parent[i] = front_node;
                     shortest_path.push(i);
                     visited[i] = true;
                     if(i == ending_vertex)
                     {
                         done = true;
                         break;
                     }
                 }
             }
         }
         delete [] visited;
         if(!done)
         {
             return NULL;
         }
         else
         {
             vector<int>* output = new vector<int>();
             int current = ending_vertex;
             output -> push_back(ending_vertex);
             while(current != starting_vertex)
             {
                 current = parent[current];
                 output -> push_back(current);
             }
             return output;
         }
    }

### 6. Is Connected - Check if the graph is totally connected ?

<b>Main Code in C++:</b>

    void check_connection(int** edges_list, int num_nodes, int starting_vertex,bool* visited_nodes)
    {
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
                check_connection(edges_list,num_nodes,i,visited_nodes);
            }
        }
    }

    bool is_connected(int** edges_list, int num_nodes, int starting_vertex)
    {
        bool* visited = new bool[num_nodes];
        for(int i=0;i<num_nodes;i++)
        {
            visited[i] = false;
        }
        check_connection(edges_list, num_nodes, starting_vertex, visited);
        int flag=0;
        for(int i=0;i<num_nodes;i++)
        {
            if(visited[i]==false)
            {
                flag++;
            }
        }
        return (flag==0);
    }
    
### 7. Transitive Closure Of A Graph Using Floyd Warshall Algorithm

<b>Main Code in C++:</b>

    /// utility function to get back the transitive closure matrix
    void transitive_closure(int** edges_list, int num_nodes)
    {
        /// creating a new 2D array
        /// copying the elements from the edges_list array
        cout << "Output Transitive Closure Graph:" << endl;
        int** output = new int*[num_nodes];
        for(int i=0;i<num_nodes;i++)
        {
            output[i] = new int[num_nodes];
            for(int j=0;j<num_nodes;j++)
            {
                output[i][j] = edges_list[i][j];
            }
        }

        /*
            i = starting node
            j = ending node
            k = intermediate node, to check if there 
                is any path from i to j through intermediate nodes
        */
        for(int k=0;k<num_nodes;k++)
        {
            for(int i=0;i<num_nodes;i++)
            {
                for(int j=0;j<num_nodes;j++)
                {
                    /*
                        output[i][j] = checks if there is direct edge between 
                                       the starting vertex and the ending vertex
                        (output[i][k] && output[k][j]) = if both true then there is 
                                        a connected graph, else it's disconnected for certain k in loop
                    */

                    output[i][j] = output[i][j] || (output[i][k] && output[k][j]);
                }
            }
        }

        /// function call to print out the transitive closure graph
        print_transitive_closure(output,num_nodes);
    }

    
    
