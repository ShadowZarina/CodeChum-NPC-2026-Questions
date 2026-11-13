# 1. CALCULATOR WITHOUT ZEROES

<img width="448" height="789" alt="image" src="https://github.com/user-attachments/assets/a119c9e1-ddf1-42f5-a8f2-2e85bdeafb97" />

## SOLUTION
```
#include<stdio.h>

int main() {
    int N, D = 0, i = 1, temp;
    
    printf("Enter number: ");
    scanf("%d", &N);
    
    while (N > 0) {
        temp = N % 10;
        N /= 10;
        // i *= 10;
        
        if (temp == 0) continue;
        // D += temp * i;
        D += temp * i;
        i *= 10;
    }

```
2. Rotting Fruit (Rotting Oranges on LC)
3. Broken Street Lamps (Substrings in LC)
4. Warmer Days (Prefix Sum in LC)
5. Longest Word in String
6. Library Fines
7. Theater Seats
8. 
    
    printf("Display: %d", D);
    
    return 0;
}
```
