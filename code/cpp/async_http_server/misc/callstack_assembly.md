```assembly
func():
        // Save caller's stack frame
        push    rbp
        // Establish func's stack frame (no locals, so no subtract from rsp)
        mov     rbp, rsp
        // Put return value in eax
        mov     eax, 1
        // Restore caller's stack frame
        pop     rbp
        // Return to the address on stack
        ret
main:
        // Save caller's stack frame
        push    rbp
        // Establish main's stack frame (16 bytes due to alignment requirements)
        mov     rbp, rsp
        sub     rsp, 16
        // Call func, storing return address on stack
        call    func()
        // Store return value in local variable
        mov     DWORD PTR [rbp-4], eax
        // Put main's return value in eax
        mov     eax, 0
        // Restore caller's stack frame
        // The leave instruction is equivalent to mov rsp, rbp; pop rbp
        leave
        // Return
        ret
```

