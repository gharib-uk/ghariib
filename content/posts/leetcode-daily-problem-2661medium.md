---
title: "Leetcode Daily Problem - 2661(Medium)"
date: 2025-01-20
---

## _LINK TO QUE_

### Problem Recap:

You are given an integer array `arr` and an `m x n` matrix `mat`. The elements of `arr` represent the order in which cells in `mat` are painted. Your task is to return the smallest index in `arr` such that either a **row** or a **column** becomes completely painted.

## Initial Setting up 40% of Question

1. **Map Elements to Their Position**: I created a map that directly linked each element of `mat` to its row and column, allowing me to easily find the location of each element in constant time.
    
2. **Use Counters for Rows and Columns**: I tracked how many cells in each row and column had been painted using two arrays, `rows` and `cols`.
    

### Then Marking Approach (Exploring Neighbors)

At first, I attempted a solution where I explored each row and column for every element in `arr`. My idea was to check if a row or column became fully painted by examining all elements in the respective row and column. However, this approach quickly ran into a **Time Limit Exceeded (TLE)** error because of excessive redundant checks!

The code used nested loops to repeatedly verify if a row or column was fully painted. This method was inefficient because each element required O(m \* n) work, leading to an overall time complexity of O(m \* n \* m \* n), which was far too slow for large inputs.  

```
bool checker(int &idx, int &idy, vector<bool> &visited, vector<vector<int>> &mat) {
    int a = mat.size(), b = mat[0].size();
    bool RL = true, UD = true;

    // Check the row (left to right)
    for (int y = 0; y < b; y++) {
        if (!visited[mat[idx][y]]) {
            RL = false;
            break;
        }
    }

    // Check the column (top to bottom)
    for (int x = 0; x < a; x++) {
        if (!visited[mat[x][idy]]) {
            UD = false;
            break;
        }
    }

    return RL || UD;
}
```

> **This approach results in TLE; we need a more efficient method to recover the time spent exploring neighbors. Introducing an early marking mechanism or a quick check to determine if all elements in either a row or a column have been visited (left-right or up-down) could significantly optimize the solution.**

### Optimized Approach (Using Row and Column Counters)

I rethought my approach and realized that I didn't need to recheck the entire row or column every time. Instead, I could **incrementally count** how many cells in each row and column had been painted, making the problem much simpler!

![SOLUTION VISULAIZTION](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxfb7bxoysiui8iyc0hrf.png)

Here’s how I optimized it:  
**Check for Complete Rows or Columns**: After each paint operation (i.e., after processing each element in `arr`), I checked if any row or column had reached its full length. If it did, I immediately returned the index of `arr` where this happened.

### Final Solution Code:

```
int firstCompleteIndex(vector<int>& arr, vector<vector<int>>& mat) {
    int a = mat.size();  // number of rows
    int b = mat[0].size();  // number of columns
    int n = arr.size();  // size of arr

    vector<int> rows(a, 0);  // row count
    vector<int> cols(b, 0);  // column count
    vector<pair<int, int>> vec(n + 1);  // to store (row, col) for each element in mat

    // Mapping each element in mat to its position
    for (int i = 0; i < a; i++) {
        for (int j = 0; j < b; j++) {
            vec[mat[i][j]] = {i, j};
        }
    }

    // Traverse through arr to paint cells
    for (int i = 0; i < n; i++) {
        int elem = arr[i];
        pair<int, int> p = vec[elem];
        int x = p.first;  // row
        int y = p.second;  // column

        // Increment painted count for the respective row and column
        rows[x]++;
        cols[y]++;

        // Check if the row or column is fully painted
        if (rows[x] == b || cols[y] == a) {
            return i;
        }
    }

    return 0;  // If no row or column is fully painted (though it shouldn't happen)
}
```

### Key Takeaways:

- Instead of checking each row and column repeatedly, I used two simple counters (`rows` and `cols`) to track the number of painted cells in each row and column.
- By mapping each number in `arr` to its position in `mat`, I could efficiently find where to increment the counters.
- This approach has a **time complexity of O(m \* n)**, which is much more efficient than the initial one.

### Why This Works:

This solution avoids redundant calculations and reduces the problem to simple counting. It ensures that we check for fully painted rows or columns in constant time, making the algorithm scalable even for large inputs.

## COMPLETE CODE

```
class Solution {
public:

    // bool checker(int&idx,int&idy,vector<bool>&visited,vector<vector<int>>& mat)
    // {

    //     int a=mat.size();
    //     int b=mat[0].size();

    //     int x=0;
    //     int y=0;

    //     // cout<<idx<<","<<idy<<endl;

    //     bool RL=true;
    //     bool UD=true;

    //     //explore right<->left
    //     while(y<b)
    //     {
    //         int elem=mat[idx][y];
    //         if(visited[elem]==false)
    //         {
    //             RL=false;
    //             break;
    //         }
    //         y++;
    //     }

    //     //explore top<->bottom
    //     while(x<a)
    //     {
    //         int elem=mat[x][idy];
    //         if(visited[elem]==false)
    //         {
    //             UD=false;
    //         }

    //         x++;
    //     }

    //     return RL | UD;
    // }

    int firstCompleteIndex(vector<int>& arr, vector<vector<int>>& mat) {

        int a=mat.size();
        int b=mat[0].size();
        int n=arr.size();
        vector<int>rows(a,0);
        vector<int>colm(b,0);

        vector<pair<int,int>>vec(n+1);

        for(int i=0 ;i<a ;i++)
        {
            for(int j=0 ;j<b ;j++)
            {
                vec[mat[i][j]]={i,j};
            }
        }

        for(int i=0 ; i< n ;i++)
        {
            int elem=arr[i];
            pair<int,int>p=vec[elem];
            int x=p.first;
            int y=p.second;
            // vector<bool>visited(n+1,false);

            rows[x]++;
            colm[y]++;
            //visited[elem]=true;

            if(rows[x]==b || colm[y]==a)
            {
                return i;
            }

            // if(checker(x,y,visited,mat)) return i;
        }

        return 0;
    }
};
```

Go to Source
