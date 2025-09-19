### LIS
```
#include<iostream>
using namespace std;

int lis(int A[], int n, int dp[100]) {
    for (int i = 0; i < n; i++) {
        dp[i] = 1;
    }
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (A[i] > A[j]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
    }
    int max_len = 0;
    for (int i = 0; i < n; i++) {
        max_len = max(max_len, dp[i]);
    }
    return max_len;
}

void printLIS(int A[], int n, int dp[100]) {
    cout << "Index: ";
    for (int i = 0; i < n; i++) {
        cout << i << " ";
    }
    cout << endl;
    cout << "Value: ";
    for (int i = 0; i < n; i++) {
        cout << A[i] << " ";
    }
    cout << endl;
    cout << "LIS:   ";
    for (int i = 0; i < n; i++) {
        cout << dp[i] << " ";
    }
    cout << endl;
}

int main() {
    int n;
    cout << "Enter the number of elements: ";
    cin >> n;
    int A[100], dp[100];
    cout << "Enter the sequence: ";
    for (int i = 0; i < n; i++) {
        cin >> A[i];
    }
    int length = lis(A, n, dp);
    cout << "LIS Length: " << length << endl;
    printLIS(A, n, dp);
    return 0;
}
```
### 0/1 knapsack greedy

```
#include <iostream>
using namespace std;

void knapsack(int n, int capacity, int weights[], int profits[]) {
    // Create DP table
    int i, j, include, exclude;
    int dp[n+1][capacity+1];
    int binary[n+1][capacity+1];
    
    // Initialize DP table
    for(i = 0; i <= n; i++) {
        for(j = 0; j <= capacity; j++) {
            if(i == 0 || j == 0) {
                dp[i][j] = 0;
                binary[i][j] = 0;
            }
            else if(weights[i-1] <= j) {
                 include = profits[i-1] + dp[i-1][j - weights[i-1]];
                 exclude = dp[i-1][j];
                
                if(include > exclude) {
                    dp[i][j] = include;
                    binary[i][j] = 1;
                } else {
                    dp[i][j] = exclude;
                    binary[i][j] = 0;
                }
            }
            else {
                dp[i][j] = dp[i-1][j];
                binary[i][j] = 0;
            }
        }
    }
    
    // 1. Output the profit DP table
    cout << "\n1. Profit DP Table:\n";
    for(i = 0; i <= n; i++) {
        for(j = 0; j <= capacity; j++) {
            cout << dp[i][j] << "\t";
        }
        cout << endl;
    }
    
    // 2. Output the binary table
    cout << "\n2. Binary Table (0=not taken, 1=taken):\n";
    for(i = 0; i <= n; i++) {
        for(j = 0; j <= capacity; j++) {
            cout << binary[i][j] << "\t";
        }
        cout << endl;
    }
    
    // 3. Output maximum profit
    cout << "\n3. Maximum Profit: " << dp[n][capacity] << endl;
    
    // 4. Find and output selected objects
    cout << "\n4. Objects selected: ";
    j = capacity;
    for(i = n; i > 0; i--) {
        if(binary[i][j] == 1) {
            cout << i << " ";
            j -= weights[i-1];
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
        cout << "Enter weight of item " << i+1 << ": ";
        cin >> weights[i];
        cout << "Enter profit of item " << i+1 << ": ";
        cin >> profits[i];
    }

    knapsack(n, capacity, weights, profits);
    
    return 0;
}

```

###  LCS
```
 
#include<iostream>
#include<cstring>
using namespace std;

int lcs(char X[], char Y[], int m, int n, int dp[100][100]) {
    for (int i = 0; i <= m; i++) {
        dp[i][0] = 0;
    }
    for (int j = 0; j <= n; j++) {
        dp[0][j] = 0;
    }
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (X[i-1] == Y[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}

void printTable(char X[], char Y[], int m, int n, int dp[100][100]) {
    cout << "  ";
    for (int j = 0; j < n; j++) {
        cout << Y[j] << " ";
    }
    cout << endl;
    for (int i = 0; i <= m; i++) {
        if (i > 0) {
            cout << X[i-1] << " ";
        } else {
            cout << "  ";
        }
        for (int j = 0; j <= n; j++) {
            cout << dp[i][j] << " ";
        }
        cout << endl;
    }
}

int main() {
    char X[100], Y[100];
    cout << "Enter first sequence: ";
    cin >> X;
    cout << "Enter second sequence: ";
    cin >> Y;
    int m = strlen(X);
    int n = strlen(Y);
    int dp[100][100] = {0};
    int length = lcs(X, Y, m, n, dp);
    cout << "LCS Length: " << length << endl;
    printTable(X, Y, m, n, dp);
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
    int i, j;
    int dp[n+1][amount+1];
    
    // Initialize DP table
    for(i = 0; i <= n; i++) {
        for(j = 0; j <= amount; j++) {
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
    for(i = 0; i <= n; i++) {
        for(j = 0; j <= amount; j++) {
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
        i = n, j = amount;
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
    int i, j;
    for(i = 0; i <= n; i++) {
        for(j = 0; j <= amount; j++) {
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
    for(i = 0; i <= n; i++) {
        for(j = 0; j <= amount; j++) {
            cout << dp[i][j] << "\t";
        }
        cout << endl;
    }
    
    // Output result
    cout << "Maximum number of ways: " << dp[n][amount] << endl;
}

int main() {
    int n, amount, i;
    
    // Take input from user
    cout << "Enter number of coin denominations: ";
    cin >> n;
    
    cout << "Enter target amount: ";
    cin >> amount;
    
    int coins[n];
    
    
    for(i = 0; i < n; i++) {
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
