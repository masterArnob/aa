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


### Coin change problem

```
#include <iostream>
using namespace std;

const int INF = 999999;

// Function to find minimum number of coins needed
void coinChangeMin(int coins[], int n, int amount) {
    int dp[n+1][amount+1];
    
    // Initialize DP table
    for(int i = 0; i <= n; i++) {
        for(int j = 0; j <= amount; j++) {
            if(j == 0) {
                dp[i][j] = 0; // 0 coins needed for amount 0
            }
            else if(i == 0) {
                dp[i][j] = INF; // No coins available, impossible
            }
            else if(coins[i-1] <= j) {
                dp[i][j] = min(dp[i-1][j], 1 + dp[i][j - coins[i-1]]);
            }
            else {
                dp[i][j] = dp[i-1][j];
            }
        }
    }
    
    // Output DP table
    cout << "\nMinimum Coins DP Table:\n";
    for(int i = 0; i <= n; i++) {
        for(int j = 0; j <= amount; j++) {
            if(dp[i][j] == INF) {
                cout << "INF\t";
            } else {
                cout << dp[i][j] << "\t";
            }
        }
        cout << endl;
    }
    
    // Output result
    if(dp[n][amount] == INF) {
        cout << "It's not possible to make amount " << amount << " with given coins." << endl;
    } else {
        cout << "Minimum coins needed: " << dp[n][amount] << endl;
        
        // Backtrack to find coins used
        cout << "Coins used: ";
        int i = n, j = amount;
        while(j > 0 && i > 0) {
            if(dp[i][j] == dp[i-1][j]) {
                i--; // Coin not used
            } else {
                cout << coins[i-1] << " ";
                j -= coins[i-1];
            }
        }
        cout << endl;
    }
}

// Function to find maximum number of ways
void coinChangeMaxWays(int coins[], int n, int amount) {
    int dp[n+1][amount+1];
    
    // Initialize DP table
    for(int i = 0; i <= n; i++) {
        for(int j = 0; j <= amount; j++) {
            if(j == 0) {
                dp[i][j] = 1; // 1 way to make amount 0 (no coins)
            }
            else if(i == 0) {
                dp[i][j] = 0; // No coins available, no ways
            }
            else if(coins[i-1] <= j) {
                dp[i][j] = dp[i-1][j] + dp[i][j - coins[i-1]];
            }
            else {
                dp[i][j] = dp[i-1][j];
            }
        }
    }
    
    // Output DP table
    cout << "\nMaximum Ways DP Table:\n";
    for(int i = 0; i <= n; i++) {
        for(int j = 0; j <= amount; j++) {
            cout << dp[i][j] << "\t";
        }
        cout << endl;
    }
    
    // Output result
    cout << "Maximum number of ways: " << dp[n][amount] << endl;
}

int main() {
    int n, amount;
    
    // Take input from user
    cout << "Enter number of coin denominations: ";
    cin >> n;
    
    cout << "Enter target amount: ";
    cin >> amount;
    
    int coins[n];
    
    
    for(int i = 0; i < n; i++) {
        cout << "Enter coin denominations " << i+1 << " : ";
        cin >> coins[i];
    }
    
    // Solve both problems
    cout << "\n=== Minimum Coins Problem ===";
    coinChangeMin(coins, n, amount);
    
    cout << "\n=== Maximum Ways Problem ===";
    coinChangeMaxWays(coins, n, amount);
    
    return 0;
}
```
