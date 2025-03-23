# XOR-Encryption

This program is takes a hard coded phrase and XORs each char with the given key '$' then prints the encrypted result.


section .data

    user_msgOne db "This is the message pre-encryption", 0
    user_msgOne_len equ $ - user_msgOne

    plaintext db 0Ah,"Hide me",0Ah, 0
    plaintext_len equ $ - plaintext
    
    key db "$", 0
    key_len equ $ - key

    encrypted db ($ - plaintext) dup(0),0Ah, 0

    user_msgTwo db "This is after encryption", 0ah, 0
    user_msgTwo_len equ $ - user_msgTwo

section .text 
    global _start

    _start:

    ;/////////////greeting msg/////////////
    mov rsi, user_msgOne
    mov edx, user_msgOne_len
    call print

    mov rsi, plaintext
    mov edx, plaintext_len
    call print

    ;/////////set up regs for encryption/////
    mov r8, plaintext
    mov al, [key]
    mov r10, encrypted
    mov rcx, plaintext_len

    call encrypt_loop

    ;//////////goodbye msgs/////////////

    mov rsi, user_msgTwo
    mov edx, user_msgTwo_len
    call print

    mov rsi, 0Ah
    mov edx, 2
    call print

    mov rsi,encrypted
    mov edx, plaintext_len
    call print

    mov eax, 60 
    xor edi, edi
    syscall

    ;//////////////encryption logic///////////////
encrypt_loop:
    mov bl, [r8]
    xor bl, al
    mov [r10], bl
    inc r8
    inc r10
    loop encrypt_loop
    ret
    

print:  ; rsi (message), edx(length) regis must be pre-filled
    mov eax, 1
    mov edi, 1
    syscall
    ret


