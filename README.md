# power-shortage-case-1
# The first line of each test case is given the number of houses m and the number of streets n. Then n lines are given information about each road x, y, z, which means that there is a two-way road between houses x and y and the distance is z meters. A city is always in the form of a connected graph, and the sum of the distances of all roads in the city is less than 231 meters. At the end of the input, two zeros are given in the first line. Print the maximum cost savings on one line for each test case! 각 테스트 케이스의 첫째 줄에는 집의 수 m과 길의 수 n이 주어진다. 이어서 n개의 줄에 각 길에 대한 정보 x, y, z가 주어지는데, 이는 x번 집과 y번 집 사이에 양방향 도로가 있으며 그 거리가 z미터라는 뜻이다. 도시는 항상 연결 그래프의 형태이고, 도시상의 모든 길의 거리 합은 231미터보다 작다. 입력의 끝에서는 첫 줄에 0이 2개 주어진다. 각 테스트 케이스마다 한 줄에 걸쳐 절약할 수 있는 최대 비용을 출력해라!
# 2) 크루스칼(Kruskal)
크루스칼이란 위에서 말한 최소 신장 트리를 구하는 알고리즘이다.
크루스칼은 greedy하게(결정의 순간마다 최선의 결정을 함으로써 최종적인 해답에 도달) 모든 정점을 최소 비용으로 연결하여 최소 신장 트리를 구한다.
크루스칼의 핵심은 모든 간선을 가중치 기준으로 오름차순으로 정렬하고,
이 간선들을 순서대로 모든 정점이 연결될 때까지 연결하는 것이다.
Union-Find 알고리즘을 이용해서 구현할 수 있고, 이를 통해 사이클이 형성되지 않으면서 모든 정점을 연결할 수 있다.
Union-Find 알고리즘은 O(1) 즉 상수 시간 복잡도를 가지기 때문에
간선을 정렬하는 로직이 전체 시간 복잡도를 좌우하게 되는데,
가장 일반적인 퀵 정렬을 예로 들면, 퀵 정렬의 시간 복잡도인 O(ElogE)가 크루스칼 알고리즘의 시간 복잡도가 된다.
//E : 간선의 개수
//V : 정점의 개수
크루스칼 알고리즘의 동작 방식
1. 그래프의 간선들을 가중치 기준 오름차순으로 정렬한다.
2-1. 정렬된 간선 리스트를 순서대로 선택하여, 간선의 정점들을 연결한다.
2-2. 이때 정점을 연결하는 것은 Union-Find의 Union으로 구현한다.
3. 만약 간선의 두 정점 a,b가 이미 연결되어 있다면 스킵한다.
4. 위의 과정을 반복하여 최소 비용의 간선들만 이용하여 모든 정점이 연결된다.

# python 크루스칼 알고리즘을 이용 
import sys
def find_parent(x, parent):
    if parent[x] != x:
        parent[x] = find_parent(parent[x], parent)
    return parent[x]
def union_parent(a, b, parent):
    a = find_parent(a, parent)
    b = find_parent(b, parent)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
if __name__ == "__main__":
    while True:
        m, n = map(int, sys.stdin.readline().split())
        if m == 0 and n == 0:
            break
        parent = [i for i in range(m)]
        edges = []
        for _ in range(n):
            x, y, z = map(int, sys.stdin.readline().split())
            edges.append((z, x, y))
        edges.sort()  # 비용을 기준으로 오름차순 정렬
        total = 0
        for i in range(len(edges)):
            z, x, y = edges[i]
            if find_parent(x, parent) != find_parent(y, parent):
                union_parent(x, y, parent)
            else:
                total += z
        print(total)
#input--> 7 11  0 1 7  0 3 5  1 2 8  1 3 9  1 4 7  2 4 5  3 4 15  3 5 6  4 5 8  4 6 9  5 6 11  0 0
#result--> 51
