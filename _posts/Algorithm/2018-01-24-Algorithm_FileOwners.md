---
layout: post
title:  "[TestDome] File Owners"
date:   2018-01-24 14:00:00 +0900
categories: Algorithm
---

Implement a group\_by\_owners function that:

- Accepts a dictionary containing the file owner name for each file name.  

- Returns a dictionary containing a list of file names for each owner name, in any order.

For example, for dictionary {'Input.txt': 'Randy', 'Code.py': 'Stan', 'Output.txt': 'Randy'} the group_by_owners function should return {'Randy': ['Input.txt', 'Output.txt'], 'Stan': ['Code.py']}.

**_Difficulty : Easy_**  
**_Time : 10min_**


```python
class FileOwners:

    @staticmethod
    def group_by_owners(files):

        owner_dict = dict()
        owner_book = list()
        for key, value in files.items():
            temp = [value, key]
            owner_book.append(temp)

        for i in owner_book:
            if i[0] not in owner_dict:
                owner_dict[i[0]] = [i[1]]
            else:
                owner_dict[i[0]].append(i[1])
        return owner_dict

files = {
    'Input.txt': 'Randy',
    'Code.py': 'Stan',
    'Output.txt': 'Randy'
}
print(FileOwners.group_by_owners(files))
```

딕셔너리 연습할 수 있는 좋은 문제!

