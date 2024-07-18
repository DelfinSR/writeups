# New Orleans

In this challenge, we are tasked with obtaining the password to proceed to the next phase.

**Disclaimer**: The purpose of this document is purely educational. It is intended to demonstrate cybersecurity techniques within a controlled environment, such as a Capture The Flag (CTF) competition. Any misuse of the information provided herein for unauthorized or illegal activities is strictly prohibited and is not the responsibility of the author. Moreover, I believe that I am not violating any rules by uploading these challenge solutions. However, if you find any content here that you think violates any rules, please contact me, and I will address the issue promptly.

Here is the **main** function in ARM assembly:

```assembly
4438:  3150 9cff      add	#0xff9c, sp
443c:  b012 7e44      call	#0x447e <create_password>
4440:  3f40 e444      mov	#0x44e4 "Enter the password to continue", r15
4444:  b012 9445      call	#0x4594 <puts>
4448:  0f41           mov	sp, r15
444a:  b012 b244      call	#0x44b2 <get_password>
444e:  0f41           mov	sp, r15
4450:  b012 bc44      call	#0x44bc <check_password>
4454:  0f93           tst	r15
4456:  0520           jnz	$+0xc <main+0x2a>
4458:  3f40 0345      mov	#0x4503 "Invalid password; try again.", r15
445c:  b012 9445      call	#0x4594 <puts>
4460:  063c           jmp	$+0xe <main+0x36>
4462:  3f40 2045      mov	#0x4520 "Access Granted!", r15
4466:  b012 9445      call	#0x4594 <puts>
446a:  b012 d644      call	#0x44d6 <unlock_door>
446e:  0f43           clr	r15
4470:  3150 6400      add	#0x64, sp
```

This ARM assembly code performs the following operations:

1. Adjusts the stack pointer.
2. Calls a function to create a password.
3. Prompts the user to enter a password.
4. Retrieves the user's input.
5. Checks if the entered password matches the created password.
6. If the password is incorrect, it prompts the user to try again.
7. If the password is correct, it grants access and unlocks the door.

Here is the **create_password** function:

```assembly
447e:  3f40 0024      mov	#0x2400, r15
4482:  ff40 5d00 0000 mov.b	#0x5d, 0x0(r15)
4488:  ff40 3500 0100 mov.b	#0x35, 0x1(r15)
448e:  ff40 4d00 0200 mov.b	#0x4d, 0x2(r15)
4494:  ff40 3e00 0300 mov.b	#0x3e, 0x3(r15)
449a:  ff40 4b00 0400 mov.b	#0x4b, 0x4(r15)
44a0:  ff40 6900 0500 mov.b	#0x69, 0x5(r15)
44a6:  ff40 4100 0600 mov.b	#0x41, 0x6(r15)
44ac:  cf43 0700      mov.b	#0x0, 0x7(r15)
44b0:  3041           ret
```

This function performs the following:

1. Loads the address `0x2400` into register `R15`.
2. Stores the byte `0x5d` at address `0x2400`.
3. Stores the byte `0x35` at address `0x2401`.
4. Stores the byte `0x4d` at address `0x2402`.
5. Stores the byte `0x3e` at address `0x2403`.
6. Stores the byte `0x4b` at address `0x2404`.
7. Stores the byte `0x69` at address `0x2405`.
8. Stores the byte `0x41` at address `0x2406`.
9. Stores the byte `0x00` at address `0x2407` (null terminator).

After the function executes, if we inspect the memory starting at `0x2400`, we get the following data:

```
2400: 5d35 4d3e 4b69 4100 0000 0000 0000 0000   ]5M>KiA.........
```

Thus, the password is:

```
]5M>KiA\x00
```

By understanding and analyzing these functions, we can determine the password required to advance to the next phase of the challenge.
