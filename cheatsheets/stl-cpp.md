# 📚 C++ STL Cheatsheet

## Containers

### Vector
```cpp
vector<int> v;
v.push_back(x);      // Add to end
v.pop_back();        // Remove from end
v.size();            // Size
v.empty();           // Is empty?
v.front(); v.back(); // First/last element
v.clear();           // Remove all
v.insert(v.begin() + i, x);  // Insert at position
v.erase(v.begin() + i);      // Erase at position
```

### String
```cpp
string s;
s.length(); s.size();      // Length
s.substr(pos, len);        // Substring
s.find("pattern");         // Find (returns npos if not found)
s += "append";             // Concatenate
s.push_back('c');          // Add char
s.pop_back();              // Remove last char
to_string(123);            // Int to string
stoi("123");               // String to int
```

### Set / Multiset
```cpp
set<int> s;
s.insert(x);         // Insert
s.erase(x);          // Remove
s.count(x);          // 0 or 1
s.find(x);           // Iterator or end()
s.lower_bound(x);    // First >= x
s.upper_bound(x);    // First > x

multiset<int> ms;    // Allows duplicates
ms.erase(ms.find(x)); // Erase one occurrence
```

### Map / Unordered Map
```cpp
map<string, int> m;
m[key] = value;      // Insert/update
m.count(key);        // 0 or 1
m.find(key);         // Iterator or end()
m.erase(key);        // Remove

unordered_map<string, int> um;  // O(1) average
```

### Stack / Queue / Deque
```cpp
stack<int> st;
st.push(x); st.pop(); st.top();

queue<int> q;
q.push(x); q.pop(); q.front(); q.back();

deque<int> dq;
dq.push_front(x); dq.push_back(x);
dq.pop_front(); dq.pop_back();
dq.front(); dq.back();
```

### Priority Queue
```cpp
// Max heap (default)
priority_queue<int> maxHeap;

// Min heap
priority_queue<int, vector<int>, greater<int>> minHeap;

pq.push(x); pq.pop(); pq.top();
```

## Algorithms

### Sorting
```cpp
sort(v.begin(), v.end());                    // Ascending
sort(v.begin(), v.end(), greater<int>());    // Descending
sort(v.begin(), v.end(), [](int a, int b) {  // Custom
    return a > b;
});
```

### Binary Search
```cpp
// Requires sorted array
binary_search(v.begin(), v.end(), x);  // Returns bool
lower_bound(v.begin(), v.end(), x);    // First >= x
upper_bound(v.begin(), v.end(), x);    // First > x
```

### Other Useful
```cpp
reverse(v.begin(), v.end());
min(a, b); max(a, b);
min_element(v.begin(), v.end());
max_element(v.begin(), v.end());
accumulate(v.begin(), v.end(), 0);      // Sum
count(v.begin(), v.end(), x);           // Count occurrences
fill(v.begin(), v.end(), x);            // Fill with x
unique(v.begin(), v.end());             // Remove consecutive duplicates
next_permutation(v.begin(), v.end());   // Next permutation
```

## Common Idioms

### Read Until EOF
```cpp
int x;
while (cin >> x) {
    // process x
}
```

### Fast I/O
```cpp
ios_base::sync_with_stdio(false);
cin.tie(NULL);
```

### Read Entire Line
```cpp
string line;
getline(cin, line);
```

### 2D Vector
```cpp
vector<vector<int>> grid(m, vector<int>(n, 0));
```

### Iterate Map
```cpp
for (auto& [key, value] : m) {
    // use key and value
}
```

### Pairs
```cpp
pair<int, int> p = {1, 2};
p.first; p.second;
auto [a, b] = p;  // Structured binding
```
