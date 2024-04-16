#Software-Engineering 

---
## # Note

Here we gonna transfer Numbers & how to use them in C

---
#### # Binary, Octal, Decimal & Hex numbers

##### # From Dec to Bin
90d = 1011010b

| Number | Number/2 | Modulo 2(%2) |
| ------ | -------- | ------------ |
| 90     | 45       | 0            |
| 45     | 22       | 1            |
| 22     | 11       | 0            |
| 11     | 5        | 1            |
| 5      | 2        | 1            |
| 2      | 1        | 0            |
| 1      | 0        | 1            |

```c
int binary = 0b1011010;
printf("binary: %d\n", binary);
// Output: binary: 90
```
---
##### # From Dec to Hex
90d = 0x5A

| Number | Number/16 | Mudolo 16 | Definition |
| ------ | --------- | --------- | ---------- |
| 90     | 5         | 10d = oxA | 90/16 = 5; 16x5 = 80; modulo=10           |
| 5      | 0         | 5d = 0x5  | 5/16 = 0; 16x0 = 0; modulo=5           |

```c
int hex = 0x5A;
printf("hexadecimal: %d\n", hex);
// Output: hexadecimal: 90
```
---
##### # From Dec to Oct
90d = 132o

| Number | Number/8 | Modulo 8 |
| ------ | -------- | -------- |
| 90     | 11       | 2        |
| 11     | 1        | 3        |
| 1      | 0        | 1        |

```c
int oct = 0132 // Leading zero = octal number
printf("octal: %d\n, oct");
// Output: octal: 90
```
---
##### # From Hex to Bin

![[Pasted image 20231104140900.png]]

---
