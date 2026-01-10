
## Longest palindrome

"abccccdd"  ->    {{a, 1}, {b, 1}, {c, 4}, {d, 2}}
oddlen = 0;
1. freq%2 == 0    -->  oddlen--    
2. freq%2 != 0     -->  oddlen++

oddlen = 1, 2, 3, 2, 3, 2, 3, 2

ans : str.length() - oddlen +1;



