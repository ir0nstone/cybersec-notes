# Python (and C#, Java)

Python has large overheads in their runtimes to provide their memory safety. Objects and allocated on a managed heap and never freed manually - instead, the **garbage collector** keeps track of references and frees objects once they become unreachable. Python also does not allow the use of pointers, and performs bounds checking on access for lists.

C# and Java are similar in operation.
