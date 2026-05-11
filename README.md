<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name:  G.SRI JANI     </h3>
<h3>Register Number:     212224060259      </h3>
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````
// A* Search Algorithm
1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor
        i) if successor is the goal, stop search
        
        ii) else, compute both g and h for successor
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
          successor.f = successor.g + successor.h

        iii) if a node with the same position as 
            successor is in the OPEN list which has a 
           lower f than successor, skip this successor

        iV) if a node with the same position as 
            successor  is in the CLOSED list which has
            a lower f than successor, skip this successor
            otherwise, add  the node to the open list
     end (for loop)
  
    e) push q on the closed list
    end (while loop)

``````

<hr>
<h2>Sample Graph I</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/b1377c3f-011a-4c0f-a843-516842ae056a)

<hr>
<h2>Sample Input</h2>
<hr>
10 14 <br>
A B 6 <br>
A F 3 <br>
B D 2 <br>
B C 3 <br>
C D 1 <br>
C E 5 <br>
D E 8 <br>
E I 5 <br>
E J 5 <br>
F G 1 <br>
G I 3 <br>
I J 3 <br>
F H 7 <br>
I H 2 <br>
A 10 <br>
B 8 <br>
C 5 <br>
D 7 <br>
E 3 <br>
F 6 <br>
G 5 <br>
H 3 <br>
I 1 <br>
J 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'F', 'G', 'I', 'J']

## Program:

```
import heapq

def a_star(graph, heuristic, start, goal):

    open_list = []
    heapq.heappush(open_list, (0, start))

    g_cost = {node: float('inf') for node in graph}
    g_cost[start] = 0

    parent = {start: None}

    closed_list = set()

    while open_list:

        f, current = heapq.heappop(open_list)

        if current == goal:
            path = []
            while current is not None:
                path.append(current)
                current = parent[current]
            path.reverse()
            return path

        closed_list.add(current)

        for neighbor, cost in graph[current]:

            if neighbor in closed_list:
                continue

            temp_g = g_cost[current] + cost

            if temp_g < g_cost.get(neighbor, float('inf')):

                parent[neighbor] = current
                g_cost[neighbor] = temp_g

                f_cost = temp_g + heuristic[neighbor]

                heapq.heappush(open_list, (f_cost, neighbor))

    return None


# -------- Predefined Input Section --------

# Graph: Example for A* from common tutorials
graph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('A', 1), ('D', 2), ('E', 5)],
    'C': [('A', 4), ('F', 1)],
    'D': [('B', 2), ('G', 3)],
    'E': [('B', 5), ('H', 2)],
    'F': [('C', 1), ('I', 3)],
    'G': [('D', 3), ('J', 1)],
    'H': [('E', 2), ('J', 1)],
    'I': [('F', 3), ('J', 1)],
    'J': [('G', 1), ('H', 1), ('I', 1)]
}

# Heuristic values (example, to be replaced by actual data)
heuristic = {
    'A': 10,
    'B': 8,
    'C': 6,
    'D': 7,
    'E': 5,
    'F': 4,
    'G': 3,
    'H': 2,
    'I': 1,
    'J': 0
}

start = 'A'
goal = 'J'

path = a_star(graph, heuristic, start, goal)

if path:
    print("Path found:", path)
else:
    print("No Path Found")
```

## Output:

<img width="550" height="78" alt="image" src="https://github.com/user-attachments/assets/a3392456-9652-4bc5-9c64-702b99423ce4" />


<hr>
<h2>Sample Graph II</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)


<hr>
<h2>Sample Input</h2>
<hr>
6 6 <br>
A B 2 <br>
B C 1 <br>
A E 3 <br>
B G 9 <br>
E D 6 <br>
D G 1 <br>
A 11 <br>
B 6 <br>
C 99 <br>
E 7 <br>
D 1 <br>
G 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'E', 'D', 'G']

## Program:
```
# Sample Input
input_str = """
6 6
A B 2
B C 1
A E 3
B G 9
E D 6
D G 1
A 11
B 6
C 99
E 7
D 1
G 0
"""

# Split the input into lines
lines = input_str.strip().split('\n')

# Parse the first line for number of nodes and edges
n_nodes, n_edges = map(int, lines[0].split())
current_line = 1

# Parse graph edges
graph = {}
for _ in range(n_edges):
    u, v, w = lines[current_line].split()
    w = int(w)
    current_line += 1

    if u not in graph:
        graph[u] = []
    if v not in graph:
        graph[v] = []

    graph[u].append((v, w))
    graph[v].append((u, w)) # Undirected graph

# Parse heuristic values
heuristic = {}
for _ in range(n_nodes):
    node, h = lines[current_line].split()
    heuristic[node] = int(h)
    current_line += 1

# Define start and goal based on the sample input
start = 'A'
goal = 'G'

# Call the a_star function
path = a_star(graph, heuristic, start, goal)

if path:
    print("Path found:", path)
else:
    print("No Path Found")
```
## Output:
<img width="420" height="46" alt="image" src="https://github.com/user-attachments/assets/256070b2-a1e8-49a0-9b1d-dc45e43855c9" />
