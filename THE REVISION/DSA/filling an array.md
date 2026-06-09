```CPP
string(1, '.')  -> "."
string(2, '.')  -> ".."
string(3, '.')  -> "..."
string(4, '.')  -> "...."
string(8, '.')  -> "........"
```



example code from n queens to fill the array of strings 

```cpp
if(row == n)

        {
            vector<string> board(n,string(n,'.'));
 
            for(int r=0;r<n;r++)

                board[r][pos[r]]='Q';

  

            ans.push_back(board);

            return;

        }
```

