---
layout: post
title: 171. Excel Sheet Column Number
categories: Algorithm
tags: 
 - leetcode
 - Math
---

[leetcode 171번 문제](https://leetcode.com/problems/excel-sheet-column-number/)

문제 이해 
* 엑셀 행 알파벳을 인덱스로 바꾸기 

해결 key 
* 26진수 체계라고 생각한다

```
    def titleToNumber(self, columnTitle: str) -> int:
        answer = 0
        a = {'A':1, 'B':2, 'C':3, 'D':4, 'E':5, 'F':6, 'G':7, 'H':8, 'I':9, 'J':10, 
             'K':11, 'L':12, 'M':13, 'N':14, 'O':15, 'P':16, 'Q':17, 'R':18, 'S':19, 'T':20,
            'U':21, 'V':22, 'W':23, 'X':24, 'Y':25, 'Z':26}
        for i, s in enumerate(columnTitle[::-1]):
            answer +=a[s]*pow(26, i)
        return answer
```





