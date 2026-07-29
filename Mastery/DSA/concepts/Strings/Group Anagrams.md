so here what we do is take a map then use key as sorted and push the unsorted then return the answer 

```cpp
class Solution {

public:

    vector<vector<string>> groupAnagrams(vector<string>& strs) {

        unordered_map<string, vector<string>> res;

        for(const auto& s:strs)

        {

            string ss=s;

            sort(ss.begin(),ss.end());

            res[ss].push_back(s);

        }

        vector<vector<string>> results;

        for(auto & pair :res)

        {

            results.push_back(pair.second);

        }

        return results;

    }

};

```