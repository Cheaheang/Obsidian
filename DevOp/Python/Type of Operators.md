**Type of Operators**
- Arithmetic Operator: -, +, , /, %, ** (exponent)
- Comparison (Relational) Operators : <, >, =
- Assignment Operators : Assign value
```python
c = 0
d = 1
c+=d # c = c + d
#output: 1
c-=d # c = c - d
#output: -1
```
- Logical Operators : or, and
```python
a = 40  
b = 50  
  
x = 2  
y = 3  
  
out = ( a < b) or ( x > y)  
print(out)  
  
out = ( a < b) or ( x > y)  
print(out)  
  
out = ( a > b) and ( x > y)  
print(out)  
  
out = ( a > b) and ( x > y)  
print(out)

out = not( a < b)  
print(out)

True
True
False
False
False
```
- Bitwise Operators
- Membership Operators
```python
# Membership Operator  
# It also check small letter or bigger letter  
first_tuple = ('Devops', 'IOT', 45, 5.6)  
ans = 'devops' in first_tuple  
print(ans)  
# output  
# False

ans = 'devops' not in first_tuple  
print(ans)
# output  
# True
```
- Identity Operators
```python
a = 12  
b = 15  
result = a is b  
print('Identity Operator', result)
# output
# Identity Operator False
result = a is not b  
print('Identity Operator', result)
# output
# Identity Operator True
```