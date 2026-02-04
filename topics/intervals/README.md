# 📏 Intervals

## Overview
Problems involving ranges, intervals, and scheduling.

## Key Concepts
- Sorting by start or end
- Merging overlapping intervals
- Finding overlaps
- Interval scheduling

## Common Patterns
1. **Merge Intervals** - Sort by start, merge overlapping
2. **Insert Interval** - Find position and merge
3. **Meeting Rooms** - Check overlaps / count rooms needed
4. **Non-Overlapping** - Greedy selection

## Templates

```cpp
// Merge Intervals
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> result;
    
    for (auto& interval : intervals) {
        if (result.empty() || result.back()[1] < interval[0]) {
            result.push_back(interval);
        } else {
            result.back()[1] = max(result.back()[1], interval[1]);
        }
    }
    return result;
}

// Check if two intervals overlap
bool overlaps(vector<int>& a, vector<int>& b) {
    return a[0] < b[1] && b[0] < a[1];
}

// Minimum meeting rooms (sweep line)
int minMeetingRooms(vector<vector<int>>& intervals) {
    vector<pair<int, int>> events;
    for (auto& i : intervals) {
        events.push_back({i[0], 1});   // start
        events.push_back({i[1], -1});  // end
    }
    sort(events.begin(), events.end());
    
    int rooms = 0, maxRooms = 0;
    for (auto& e : events) {
        rooms += e.second;
        maxRooms = max(maxRooms, rooms);
    }
    return maxRooms;
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- Always consider sorting by start or end
- Sweep line for counting overlaps at any point
- Two intervals [a,b] and [c,d] overlap if a < d && c < b
