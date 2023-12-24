# Stack



## Theory

https://www.youtube.com/playlist?list=PL_z_8CaSLPWdeOezg68SKkeLN4-T_jNHd


## Questions

1. Nearest Greatest Element towards left/right
2. Nearest Smallest  Element towards left/right
3. Stock span
4. Max area under histogram
5. Max area of rectangle in a binary matrix
6. Rainwater trapping
7. Min Stack
8. Celebrity Problem
9. Valid Parentheses https://leetcode.com/problems/valid-parentheses
10. Longest Valid Paratheses* https://leetcode.com/problems/longest-valid-parentheses
11. Simplify Path* https://leetcode.com/problems/simplify-path/
12. Polish Notation https://leetcode.com/problems/evaluate-reverse-polish-notation
13. Basic Calculator** https://leetcode.com/problems/basic-calculator
14. Basic Calculator II https://leetcode.com/problems/basic-calculator-ii/
15. Longest Absolute File path https://leetcode.com/problems/longest-absolute-file-path/

``` 
public int lengthLongestPath(String input) {
        if (input == null || input.length() == 0) {
            return 0;
        }
        String[] tokens = input.split("\n");
        int[] dirs = new int[tokens.length];
        int max = 0;
        int curr;
        for (int i = 0; i < tokens.length; i ++) {
            String s = tokens[i];
            int count = s.lastIndexOf("\t") + 1;
            s = s.substring(count);
            if (s.indexOf(".") != -1) {
                //file
                int cand = s.length();
                if (count != 0) {
                    cand = dirs[count - 1] + s.length();
                }
                max = Math.max(max, cand);
            }
            else {
                //folder
                dirs[count] = s.length() + 1;
                if (count != 0) {
                    dirs[count] += dirs[count - 1];
                }
            }
        }
        return max;
        
    }
} 
```

16. Remove K digits to get max https://leetcode.com/problems/remove-k-digits/
17. Parsing a boolean expression https://leetcode.com/problems/parsing-a-boolean-expression/



## Extras 

Prefix , postfix & infix conversion
