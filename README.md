BFS(Graph, StartNode)

    // Graph is represented as adjacency list
    CREATE empty queue Q
    CREATE empty set Visited

    ADD StartNode to Visited
    ENQUEUE StartNode into Q

    WHILE Q is not empty DO
        CurrentNode ← DEQUEUE Q
        VISIT CurrentNode

        FOR each Neighbor of CurrentNode in Graph DO
            IF Neighbor not in Visited THEN
                ADD Neighbor to Visited
                ENQUEUE Neighbor into Q
            END IF
        END FOR
    END WHILE

END BFS
DFS(Graph, StartNode)

    CREATE empty set Visited

    DFS_Visit(StartNode)

    FUNCTION DFS_Visit(Node)
        ADD Node to Visited
        VISIT Node

        FOR each Neighbor of Node in Graph DO
            IF Neighbor not in Visited THEN
                DFS_Visit(Neighbor)
            END IF
        END FOR
    END FUNCTION

END DFS
WATER_JUG(Jug1_Capacity, Jug2_Capacity, Target)

    CREATE empty set Visited
    CREATE empty queue Q

    INITIAL_STATE ← (0, 0)
    ENQUEUE INITIAL_STATE into Q
    ADD INITIAL_STATE to Visited

    WHILE Q is not empty DO
        (x, y) ← DEQUEUE Q

        IF x = Target OR y = Target THEN
            PRINT "Target reached"
            STOP
        END IF

        GENERATE all possible next states:
            (Jug1_Capacity, y)          // Fill Jug1
            (x, Jug2_Capacity)          // Fill Jug2
            (0, y)                      // Empty Jug1
            (x, 0)                      // Empty Jug2
            (x - min(x, Jug2_Capacity - y), y + min(x, Jug2_Capacity - y))
            (x + min(y, Jug1_Capacity - x), y - min(y, Jug1_Capacity - x))

        FOR each NewState DO
            IF NewState not in Visited THEN
                ADD NewState to Visited
                ENQUEUE NewState into Q
            END IF
        END FOR
    END WHILE

    PRINT "Target not reachable"

END WATER_JUG
UCS(Graph, StartNode, GoalNode)

    // Graph is represented as adjacency list with costs
    CREATE empty priority queue PQ
    CREATE empty set Visited

    INSERT (StartNode, cost = 0) into PQ

    WHILE PQ is not empty DO
        (CurrentNode, CurrentCost) ← REMOVE node with minimum cost from PQ

        IF CurrentNode = GoalNode THEN
            PRINT "Goal reached with cost", CurrentCost
            STOP
        END IF

        IF CurrentNode not in Visited THEN
            ADD CurrentNode to Visited

            FOR each Neighbor of CurrentNode in Graph DO
                Cost ← CurrentCost + cost(CurrentNode, Neighbor)
                INSERT (Neighbor, Cost) into PQ
            END FOR
        END IF

    END WHILE

    PRINT "Goal not reachable"

END UCS
A_Star(Graph, StartNode, GoalNode)

    // Graph contains cost and heuristic values
    CREATE empty priority queue OPEN
    CREATE empty set CLOSED

    INSERT (StartNode, g = 0, f = h(StartNode)) into OPEN

    WHILE OPEN is not empty DO
        CurrentNode ← REMOVE node with minimum f from OPEN

        IF CurrentNode = GoalNode THEN
            PRINT "Goal reached"
            STOP
        END IF

        ADD CurrentNode to CLOSED

        FOR each Neighbor of CurrentNode in Graph DO
            IF Neighbor not in CLOSED THEN
                g_new ← g(CurrentNode) + cost(CurrentNode, Neighbor)
                f_new ← g_new + h(Neighbor)

                INSERT (Neighbor, g_new, f_new) into OPEN
            END IF
        END FOR

    END WHILE

    PRINT "Goal not reachable"

END A_Star
GREEDY_BEST_FIRST_SEARCH(Graph, StartNode, GoalNode)

    // Uses only heuristic h(n)
    CREATE empty priority queue OPEN
    CREATE empty set Visited

    INSERT (StartNode, h(StartNode)) into OPEN

    WHILE OPEN is not empty DO
        CurrentNode ← REMOVE node with minimum h from OPEN

        IF CurrentNode = GoalNode THEN
            PRINT "Goal reached"
            STOP
        END IF

        ADD CurrentNode to Visited

        FOR each Neighbor of CurrentNode in Graph DO
            IF Neighbor not in Visited THEN
                INSERT (Neighbor, h(Neighbor)) into OPEN
            END IF
        END FOR
    END WHILE

    PRINT "Goal not reachable"

END GREEDY_BEST_FIRST_SEARCH
ALPHA_BETA(Node, Depth, Alpha, Beta, IsMaxPlayer)

    IF Depth = 0 OR Node is terminal THEN
        RETURN value of Node
    END IF

    IF IsMaxPlayer = TRUE THEN
        Best ← -∞

        FOR each Child of Node DO
            Value ← ALPHA_BETA(Child, Depth-1, Alpha, Beta, FALSE)
            Best ← max(Best, Value)
            Alpha ← max(Alpha, Best)

            IF Beta ≤ Alpha THEN
                BREAK   // Beta Cutoff
            END IF
        END FOR

        RETURN Best

    ELSE
        Best ← +∞

        FOR each Child of Node DO
            Value ← ALPHA_BETA(Child, Depth-1, Alpha, Beta, TRUE)
            Best ← min(Best, Value)
            Beta ← min(Beta, Best)

            IF Beta ≤ Alpha THEN
                BREAK   // Alpha Cutoff
            END IF
        END FOR

        RETURN Best
    END IF

END ALPHA_BETA
DECISION_TREE(TrainingData, Attributes)

    IF all examples belong to same class THEN
        RETURN Leaf with that class
    END IF

    IF Attributes is empty THEN
        RETURN Leaf with majority class
    END IF

    SELECT best Attribute using Information Gain
    CREATE Decision Node with selected Attribute

    FOR each value v of selected Attribute DO
        CREATE subset Data_v from TrainingData
        IF Data_v is empty THEN
            ATTACH Leaf with majority class
        ELSE
            ATTACH DECISION_TREE(Data_v, remaining Attributes)
        END IF
    END FOR

    RETURN Decision Node

END DECISION_TREE

