# Sidney
In this challenge, we are tasked with retrieving the password to advance to the next phase.

**Disclaimer**: The purpose of this document is purely educational. It is intended to demonstrate cybersecurity techniques within a controlled environment, such as a Capture The Flag (CTF) competition. Any misuse of the information provided herein for unauthorized or illegal activities is strictly prohibited and is not the responsibility of the author. Moreover, I believe that I am not violating any rules by uploading these challenge solutions. However, if you find any content here that you think violates any rules, please contact me, and I will address the issue promptly. Before any usage of these write-ups, please review the rules on the official CTF site. Before any usage of these write-ups, please review the rules on the official CTF site.

Here is the **main** function:

```assembly
4438:  3150 9cff      add	#0xff9c, sp
443c:  3f40 b444      mov	#0x44b4 "Enter the password to continue.", r15
4440:  b012 6645      call	#0x4566 <puts>
4444:  0f41           mov	sp, r15
4446:  b012 8044      call	#0x4480 <get_password>
444a:  0f41           mov	sp, r15
444c:  b012 8a44      call	#0x448a <check_password>
4450:  0f93           tst	r15
4452:  0520           jnz	$+0xc <main+0x26>
4454:  3f40 d444      mov	#0x44d4 "Invalid password; try again.", r15
4458:  b012 6645      call	#0x4566 <puts>
445c:  093c           jmp	$+0x14 <main+0x38>
445e:  3f40 f144      mov	#0x44f1 "Access Granted!", r15
4462:  b012 6645      call	#0x4566 <puts>
4466:  3012 7f00      push	#0x7f
446a:  b012 0245      call	#0x4502 <INT>
446e:  2153           incd	sp
4470:  0f43           clr	r15
4472:  3150 6400      add	#0x64, sp
```

This is clearly not Intel assembly. Essentially, this function prompts the user for a password and checks if the entered password meets specific criteria defined in the `check_password` function.

Here is the **check_password** function:

```assembly
448a:  bf90 2e37 0000 cmp	#0x372e, 0x0(r15)
4490:  0d20           jnz	$+0x1c <check_password+0x22>
4492:  bf90 5575 0200 cmp	#0x7555, 0x2(r15)
4498:  0920           jnz	$+0x14 <check_password+0x22>
449a:  bf90 5c31 0400 cmp	#0x315c, 0x4(r15)
44a0:  0520           jnz	$+0xc <check_password+0x22>
44a2:  1e43           mov	#0x1, r14
44a4:  bf90 3c4b 0600 cmp	#0x4b3c, 0x6(r15)
44aa:  0124           jz	$+0x4 <check_password+0x24>
44ac:  0e43           clr	r14
44ae:  0f4e           mov	r14, r15
44b0:  3041           ret
```

This function verifies that the entered password matches the expected one by comparing each segment (two bytes at a time) with predefined values. These values are ASCII representations of the password characters.

To pass the `check_password` function, the password must match the following ASCII values:
- `0x372e` ('.7')
- `0x7555` ('Uu')
- `0x315c` ('\1')
- `0x4b3c` ('<K')

Thus, the correct password is:
```
.7Uu\1<K
```
