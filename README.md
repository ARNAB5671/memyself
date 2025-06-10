Dynamic programming is a method for solving a complex problem by breaking it down into a collection of simpler subproblems.
[Problem]

N pairs of numbers are given.

The pair consists of two numbers. Assume that the first number is x and the second number is y and denote them as (x, y). Only 1 or 2 can be given as x. Only an integer greater than or equal to 1 and less than or equal to 100 can be given as y.

When you select some of the N pairs of numbers and add all the x values of the selected pairs, the added value must be bigger than or equal to the sum of the y values of the unselected pairs.

For example, if N is 5, you are given with (x0, y0), (x1, y1), (x2, y2), (x3, y3), (x4, y4). When you select the 0th, 2nd and 4th pair, x0+x2+x4 ≥ y1+y3 must be satisfied.

When there are N pairs of numbers, among the methods to select the pairs conforming to the above conditions, find the cases in which the sum of x values of the selected pairs can be the minimum and output this minimum sum.

[Constraints]
1) N is an integer greater than or equal to 1 and less than or equal to 1,000. (1 ≤ N ≤ 1,000)
2) When the pair of numbers is given as (x, y), x and y are integers that satisfy 1 ≤ x ≤ 2 and 1 ≤ y ≤ 100.
Input	Output
2
5
50 10 100 20 40
30 50 70 10 60
7
10 30 10 50 100 20 40
20 40 30 50 60 20 80
#1 260
#2 290
C / C++
JAVA
Python
MAX_X = 2
MAX_N = 100

N = 0
num_pair = None
dp = [[0] * MAX_N for j in range(MAX_X * MAX_N)]


def max(a, b):
    if a > b:
        return a
    return b


def solve():
    global N, num_pair, dp
    ans = 0

    for i in range(0, N):
        ans += num_pair[i][0]

    for i in range(0, N):
        for j in range(0, ans):
            dp[i][j] = -1

    dp[0][ans] = 0
    dp[0][ans - num_pair[0][0] + num_pair[0][1]] = num_pair[0][0]

    for i in range(1, N):
        sum = num_pair[i][0] + num_pair[i][1]
        for j in range(0, ans + 1):
            if dp[i - 1][j] != -1:
                diff = j - sum
                if diff >= 0:
                    dp[i][diff] = max(dp[i][diff], dp[i - 1][j] + num_pair[i][0])

                dp[i][j] = dp[i - 1][j]

    max_value = -1
    for i in range(0, N):
        for j in range(0, ans + 1):
            max_value = max(max_value, dp[i][j])

    return ans - max_value


def main():
    global N, num_pair
    T = int(input())

    for test_case in range(1, T + 1):
        N = int(input())
        num_pair = [[int(num) for num in input().split()] for _ in range(N)]
        print("#" + str(test_case) + " " + str(solve()))


if __name__ == "__main__":
    main()
