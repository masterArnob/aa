### 0/1 knapsack greedy

```
#include <iostream>
using namespace std;

void solveKnapsack(int n, int capacity, int weights[], int profits[]) {
    // Create DP table
    int dp[n+1][capacity+1];
    int binary[n+1][capacity+1];
    
    // Initialize DP table
    for(int i = 0; i <= n; i++) {
        for(int w = 0; w <= capacity; w++) {
            if(i == 0 || w == 0) {
                dp[i][w] = 0;
                binary[i][w] = 0;
            }
            else if(weights[i-1] <= w) {
                int include = profits[i-1] + dp[i-1][w - weights[i-1]];
                int exclude = dp[i-1][w];
                
                if(include > exclude) {
                    dp[i][w] = include;
                    binary[i][w] = 1;
                } else {
                    dp[i][w] = exclude;
                    binary[i][w] = 0;
                }
            }
            else {
                dp[i][w] = dp[i-1][w];
                binary[i][w] = 0;
            }
        }
    }
    
    // 1. Output the profit DP table
    cout << "\n1. Profit DP Table:\n";
    for(int i = 0; i <= n; i++) {
        for(int w = 0; w <= capacity; w++) {
            cout << dp[i][w] << "\t";
        }
        cout << endl;
    }
    
    // 2. Output the binary table
    cout << "\n2. Binary Table (0=not taken, 1=taken):\n";
    for(int i = 0; i <= n; i++) {
        for(int w = 0; w <= capacity; w++) {
            cout << binary[i][w] << "\t";
        }
        cout << endl;
    }
    
    // 3. Output maximum profit
    cout << "\n3. Maximum Profit: " << dp[n][capacity] << endl;
    
    // 4. Find and output selected objects
    cout << "\n4. Objects selected: ";
    int w = capacity;
    for(int i = n; i > 0; i--) {
        if(binary[i][w] == 1) {
            cout << i << " ";
            w -= weights[i-1];
        }
    }
    cout << endl;
}

int main() {
    int n, capacity;
    
    // Take input from user in main function
    cout << "Enter number of items: ";
    cin >> n;
    
    cout << "Enter knapsack capacity: ";
    cin >> capacity;
    
    int weights[n], profits[n];
    
    
    for(int i = 0; i < n; i++) {
        cout << "Enter weights of items: " << i+1 <<" : ";
        cin >> weights[i];
        cout << "Enter profits of items: " << i+1 << " : ";
        cin >> profits[i];
    }

    
    // Call the knapsack solving function
    solveKnapsack(n, capacity, weights, profits);
    
    return 0;
}
```
