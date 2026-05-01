.MODEL SMALL
 
.STACK 100H

.DATA  


; welcome page banner

m1 db 10,13,"*****************************************$"

m2 db 10,13,"***   Welcome to 360-Degree Corner    ***$"    

m3 db 10,13,"*****************************************$" 

m4 db 10,13, "Enter < 1 > for admin login page $" 
m5 db 10,13, "Enter < 2 > for customer page $" 
m6 db 10,13, "Enter your input $"
m7 db 10,13, "Invalid input!! $" 
m8 db 10,13, "Enter < 3 > to exit $"

; admin page banner

a1 db 10,13, "===========ADMIN LOG-IN PANEL==========$"
a2 db 10,13, "Enter Password << 4-Digit >> $"
a3 db 10,13, "Password Incorrect!! $"
a4 db 10,13, "Password Correct!! $" 


pas dw ? 

c_pas dw 5665


; warehouse page banner

w1 db 10,13, "You are inside warehouse $"
w2 db 10,13, "Enter Product code <2-digit> $" 
w3 db 10,13, "Enter new quantity <2-digit> $" 
w4 db 10,13, "Enter < 1 > to see products in warehouse $"
w5 db 10,13, "Enter < 2 > to change quantity $"
w6 db 10,13, "Enter < 3 > to go back to welcome panel $"

r1 db 10,13, "Enter < 4 > to reset admin password $"

w7 db 10,13, "Enter < 5 > to exit$"
                      
pr db ?  

cal db 1 ; print product flag

;utilizing m6,m7 for invalid inputs


; reset password

reset dw ?
confirm dw ?

  
r2 db 10,13, "Enter New 4 digit Password $"
r3 db 10,13, "Re-Enter to Confirm Password $"
r4 db 10,13, "Password Reset has been SUCCESSFUL!!$"
r5 db 10,13, "Password Reset has been UNSUCCESSFUL!!$"
r6 db 10,13, "Redirected to login-panel$"

 

; product list msgs

p db 10,13, "  $"

q db "Product quantity $" 

la db ?
f db ?

p1 db 10,13, "Product Name : Eggs ; Price 15 TK ; Product code < 01 > $" 

egg db 20 ; quantitiy of eggs 

p2 db 10,13, "Product Name : Tomato ; Price 25 TK ; Product code < 02 > $" 

tomato db 40 ; quantitiy of tomato 

p3 db 10,13, "Product Name : Potato ; Price 20 TK ; Product code < 03 > $" 

potato db 20 ; quantitiy of tomato

p4 db 10,13, "Product Name : Onion ; Price 20 TK ; Product code < 04 > $" 

onion db 40 ; quantitiy of tomato

p5 db 10,13, "Product Name : Garlic ; Price 30 TK ; Product code < 05 > $" 

garlic db 30 ; quantitiy of tomato   

p6 db 10,13,  "Not Enough in our stock SORRY!!$"


; customer page banner 

c1 db 10,13, "360-Degree Shopping Corner $" 
c2 db 10,13, "Enter < 1 > to add items to cart $"
c3 db 10,13, "Enter < 2 > delete cart $" 
c6 db 10,13, "Enter < 3 > to place order $" 
c7 db 10,13, "Enter < 4 > to see products list again$"
c11 db 10,13,"Enter < 5 > to go back to welcome panel$"
c4 db 10,13, "Enter < 6 > to Exit $"

c8 db 10,13, "Cart empty !! can not place order or delete cart$" 
c9 db 10,13, "Cart has been deleted $!!"

c10 db 10,13, "Your order has been placed $"  

c12 db 10,13, "Cart is full!! can not add new items $"


;    use w2 

c5 db 10,13, "Enter quantity you want to buy $"


; cart info 

item_arr db 10 dup (0)

quant_arr db 10 dup (0)

arr_len db 0


; billing info 

bill dw 0  

divisor1 dw ?

ten1 dw ?

divisor2 dw ?

ten2 dw ?

b1  db 10,13,"Your total bill in BDT is $ "
b2  db 10,13,"Your total converted bill in USD is $ "


usd dw 0

usd_sents dw 0 


; order check

o1 db 10,13, "Enter < 1 > to pay in BDT $ "
o2 db 10,13, "Enter < 2 > to pay in USD $"

o3 db " Dollars and $" 
o4 db " Sents $"   


o5 db 10,13,"Enter < 1 > to shop again $ "
o6 db 10,13, "Enter < 2 > to exit $"


;digit check

dig db 1 ; 1 if called from warehouse 2 if called from cart 3 if for reset pas

; exit 

e1 db 10,13, " ^-^ THANK YOU for supporting us ^-^ $ "




.CODE
MAIN PROC

;iniitialize DS

MOV AX,@DATA
MOV DS,AX
 
; enter your code here 

; welcome page
 
wp:
 
mov ah,9
lea dx,m1
int 21h   

mov ah,9
lea dx,m1
int 21h 

mov ah,9
lea dx,m2
int 21h

mov ah,9
lea dx,m3
int 21h 

mov ah,9
lea dx,m3
int 21h 

mov ah,9
lea dx,m4
int 21h

mov ah,9
lea dx,m5
int 21h

mov ah,9
lea dx,m8
int 21h
       

welcome:       

    mov ah,9
    lea dx,m6
    int 21h
    
    
    
    mov ah,1
    int 21h
    
    sub al,30h
    
    cmp al,1
    je admin
    
    cmp al,2
    je customer 
    
    cmp al,3
    je exit
    
    ;invalid input
    
    mov ah,9
    lea dx,m7
    int 21h
    
    jmp welcome


admin:  

    mov ah,9
    lea dx,a1
    int 21h 
    
    
  pas_prompt:
    
    mov ah,9
    lea dx,a2
    int 21h
    
    ; 4 digit password input 
    
    mov ax,0
    
    mov ah,1
    int 21h
    
    sub al,30h 
    
    mov ah,0    ; word mul , ax whole needed , so flushing ah
    
    mov bx,1000
    mul bx
    mov pas,ax 
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    mov bl,100
    mul bl
    add pas,ax 
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    mov bl,10
    mul bl
    add pas,ax
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    mov bl,1
    mul bl
    add pas,ax 
    
    
    ; password validity check  
    
    mov bx, c_pas
    
    cmp pas,bx
    
    je warehouse
    jne inc_pass
    
    
  warehouse:
  
    mov bl,1
    mov cal,bl ; call = 1 means printing products called from admin/warehouse page 
    
    mov dig,bl ; dig = 1 means not_digit called from warehouse
    
    
    mov ah,9
    lea dx,w1
    int 21h
    
    mov ah,9
    lea dx,w4
    int 21h 

    mov ah,9
    lea dx,w5
    int 21h
    
    
    mov ah,9
    lea dx,w6
    int 21h 
    
    mov ah,9
    lea dx,r1
    int 21h
    
    mov ah,9
    lea dx,w7
    int 21h 
    
    mov ah,9
    lea dx,m6
    int 21h    
    
    mov ah,1
    int 21h
    sub al,30h 
    
    mov ah,0
    
    cmp al,1
    je print_products
    
    cmp al,2
    je change
    
    cmp al,3
    je wp
    
    cmp al,4
    je reset_pas
    
    
    cmp al,5
    je exit
    
    ; invalid input 
    
    mov ah,9
    lea dx,m7
    int 21h
    jmp warehouse
    
    
    ; asking admin what they want to change 
   change:
    mov ah,9
    lea dx,w2
    int 21h 
    
    ; taking input from admin 
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    mov bl,10
    mul bl
    mov pr,al
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    mov bl,1
    mul bl
    add pr,al 
    
     
    mov cl,pr
    
    cmp cl, 1
    je cng_egg

    cmp cl, 2
    je cng_tomato
    
    cmp cl,3
    je cng_potato
    
    cmp cl,4
    je cng_onion
    
    cmp cl,5
    je cng_garlic
    
    ; didnt match any 
    mov ah,9
    lea dx,m7
    int 21h
    
    jmp change
   
   cng_egg:
   
    mov ah,9
    lea dx,w3
    int 21h
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit   
    
    mov bl,10
    mul bl
    mov egg,al
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit
    
    mov bl,1
    mul bl
    add egg,al
    
    jmp warehouse  
   
   cng_tomato:
   
    mov ah,9
    lea dx,w3
    int 21h
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h 
    
        cmp al,9
        ja not_digit 
        
    
    mov bl,10
    mul bl
    mov tomato,al
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h 
    
        cmp al,9
        ja not_digit 
        
    mov bl,1
    mul bl
    add tomato,al 
    
    jmp warehouse 
    
   cng_potato:
   
    mov ah,9
    lea dx,w3
    int 21h
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 

    mov bl,10
    mul bl
    mov potato,al
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 
        
    mov bl,1
    mul bl
    add potato,al 
    
    jmp warehouse
   
   cng_onion:
   
    mov ah,9
    lea dx,w3
    int 21h
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 
        
    mov bl,10
    mul bl
    mov onion,al
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 
        
    mov bl,1
    mul bl
    add onion,al 
    
    jmp warehouse
   
   cng_garlic:
   
    mov ah,9
    lea dx,w3
    int 21h
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 
        
    mov bl,10
    mul bl
    mov garlic,al
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 
        
    mov bl,1
    mul bl
    add garlic,al 
    
    jmp warehouse
     
           
  inc_pass:
    
    
    mov ah,9
    lea dx,a3
    int 21h     
    jmp pas_prompt
    
    
  reset_pas:
  
    mov dig,3

    mov ah,9
    lea dx,r2
    int 21h
    
    
    ; 4 digit password input 
    
    mov ax,0
    
    mov ah,1
    int 21h
    
    sub al,30h 
    
        cmp al,9
        ja not_digit
    
    mov ah,0    ; word mul , ax whole needed , so flushing ah
    
    mov bx,1000
    mul bx
    mov reset,ax 
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit
        
    mov bl,100
    mul bl
    add reset,ax 
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit
    
    mov bl,10
    mul bl
    add reset,ax
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit
    
    mov bl,1
    mul bl
    add reset,ax
    
    mov ah,9
    lea dx,r3
    int 21h
    
    
    mov ax,0
    
    mov ah,1
    int 21h
    
    sub al,30h
    
        cmp al,9
        ja not_digit 
    
    mov ah,0    
    
    mov bx,1000
    mul bx
    mov confirm,ax 
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit
    
    mov bl,100
    mul bl
    add confirm,ax 
    
    mov ax,0
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit
        
    mov bl,10
    mul bl
    add confirm,ax
    
    mov ax,0          
    
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit
        
    mov bl,1
    mul bl
    add confirm,ax  
    
    
    ; compare them
    
    mov ax, confirm
    mov bx, reset
    
    cmp ax,bx
    je done_reset
    jne not_done_reset
    
    
    done_reset:
    
        
        mov c_pas,ax
        
        mov ah,9
        lea dx,r4
        int 21h
        mov ah,9
        lea dx,r6
        int 21h  
        
        jmp admin
        
        
    not_done_reset:
    
        mov ah,9
        lea dx,r5
        int 21h
        mov ah,9
        lea dx,r6
        int 21h  
        
        jmp admin    
    
    
    
    
customer:

    mov ah,9  
    lea dx,p
    int 21h 

    mov ah,9
    lea dx,c1
    int 21h
    
    mov bl,2
    mov cal,bl ; call = 2 means printing products called from customer page 
    mov dig,bl ; dig = 2 means not_digit called from customer cart
    
    jmp print_products
    
    choice:

    mov ah,9
    lea dx,c2
    int 21h
    
    mov ah,9
    lea dx,c3
    int 21h
    
    mov ah,9
    lea dx,c6
    int 21h 

    mov ah,9
    lea dx,c7
    int 21h
    
    mov ah,9
    lea dx,c11
    int 21h
    
    mov ah,9
    lea dx,c4
    int 21h
    
    mov ah,9
    lea dx,m6
    int 21h
    
    
    mov ah,1
    int 21h
    
    sub al,30h 
    
    
    cmp al,1
    je add_to_cart
    
    cmp al,2
    je delete_cart
    
    cmp al,3
    je  place_order
    
    cmp al,4
    je print_products
    
    cmp al,5
    je wp
    
    cmp al,6
    je exit
    

    mov ah,9
    lea dx,m7
    int 21h
    
    jmp choice
    
    


print_products:
    
    mov ah,9  
    lea dx,p
    int 21h     

    mov ah,9
    lea dx,p1
    int 21h
    
    mov ah,9
    lea dx,q
    int 21h 
     
    
    mov ax,0
    mov al,egg
    mov bl,10
    div bl 
    
    mov la,ah
    mov f,al
    
    
    mov ah,2
    mov dl,f
    add dl,30h
    int 21h 
    
    mov ah,2
    mov dl,la
    add dl,30h
    int 21h 
    

    ; printing tomato
    
    mov ah,9
    lea dx,p
    int 21h     

    mov ah,9
    lea dx,p2
    int 21h
    
    mov ah,9
    lea dx,q
    int 21h 
     
    
    mov ax,0
    mov al,tomato
    mov bl,10
    div bl 
    
    mov la,ah
    mov f,al
    
    
    mov ah,2
    mov dl,f
    add dl,30h
    int 21h 
    
    mov ah,2
    mov dl,la
    add dl,30h
    int 21h
    
    ; printing potato
    
    mov ah,9
    lea dx,p
    int 21h     

    mov ah,9
    lea dx,p3
    int 21h
    
    mov ah,9
    lea dx,q
    int 21h 
     
    
    mov ax,0
    mov al,potato
    mov bl,10
    div bl 
    
    mov la,ah
    mov f,al
    
    
    mov ah,2
    mov dl,f
    add dl,30h
    int 21h 
    
    mov ah,2
    mov dl,la
    add dl,30h
    int 21h
    
    ; printing onion
    
    mov ah,9
    lea dx,p
    int 21h     

    mov ah,9
    lea dx,p4
    int 21h
    
    mov ah,9
    lea dx,q
    int 21h 
     
    
    mov ax,0
    mov al,onion
    mov bl,10
    div bl 
    
    mov la,ah
    mov f,al
    
    
    mov ah,2
    mov dl,f
    add dl,30h
    int 21h 
    
    mov ah,2
    mov dl,la
    add dl,30h
    int 21h 
    
    
    ; printing garlic
    
    mov ah,9
    lea dx,p
    int 21h     

    mov ah,9
    lea dx,p5
    int 21h
    
    mov ah,9
    lea dx,q
    int 21h 
     
    
    mov ax,0
    mov al,garlic
    mov bl,10
    div bl 
    
    mov la,ah
    mov f,al
    
    
    mov ah,2
    mov dl,f
    add dl,30h
    int 21h 
    
    mov ah,2
    mov dl,la
    add dl,30h
    int 21h
    
    
    ; checking who called print product
    mov bl,cal
    
    cmp cal,1
    je warehouse 
    
    jne choice

 
 
 add_to_cart:
 
 
    cmp arr_len,10
    je cart_full
    
    ; Prompt for product code
    mov ah,9
    lea dx,w2
    int 21h

    ; Input product code
    mov ax,0
    mov ah,1
    int 21h
    sub al,30h
    mov bl,10
    mul bl
    mov pr,al
    mov ax,0
    mov ah,1
    int 21h
    sub al,30h
    mov bl,1
    mul bl
    add pr,al

    ; Prompt for quantity
    mov ah,9
    lea dx,c5
    int 21h

    ; Input quantity
    mov ax,0
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 
        
    mov bl,10
    mul bl
    mov la,al
    
    
    mov ax,0
    mov ah,1
    int 21h
    sub al,30h
    
        cmp al,9
        ja not_digit 
        
    mov bl,1
    mul bl
    add la,al

    ; Validate and update stock
    cmp pr,1
    je validate_egg
    cmp pr,2
    je validate_tomato
    cmp pr,3
    je validate_potato
    cmp pr,4
    je validate_onion
    cmp pr,5
    je validate_garlic

    ; Invalid product code
    mov ah,9
    lea dx,m7
    int 21h
    jmp add_to_cart

validate_egg: 
    
    mov bl,la
    mov al,egg
    cmp bl,al
    jg not_enough_stock
    sub egg,bl
    
    mov dx,0
    mov dl,arr_len
    
    mov si,dx
    mov item_arr[si],1
    mov quant_arr[si],bl
    inc arr_len
    jmp choice

validate_tomato:
 
    mov bl,la
    mov al,tomato

    cmp bl,al
    jg not_enough_stock
    sub tomato,bl
    
    mov dx,0
    mov dl,arr_len
    
    mov si,dx
    mov item_arr[si],2
    mov quant_arr[si],bl
    inc arr_len
    jmp choice

validate_potato:

    mov bl,la
    mov al,potato

    cmp bl,al
    jg not_enough_stock
    sub potato,bl
    
    mov dx,0
    mov dl,arr_len
    
    mov si,dx
    mov item_arr[si],3
    mov quant_arr[si],bl
    inc arr_len
    jmp choice

validate_onion:
    mov bl,la
    mov al,onion

    cmp bl,al
    jg not_enough_stock
    sub onion,bl 
    
    mov dx,0
    mov dl,arr_len
    
    mov si,dx
    mov item_arr[si],4
    mov quant_arr[si],bl
    inc arr_len
    jmp choice

validate_garlic:
    mov bl,la
    mov al,garlic

    cmp bl,al
    jg not_enough_stock
    sub garlic,bl
    mov dx,0
    mov dl,arr_len
    
    mov si,dx
    mov item_arr[si],5
    mov quant_arr[si],bl
    inc arr_len
    jmp choice

not_enough_stock:
    mov ah,9
    lea dx,p6
    int 21h
    jmp add_to_cart
    
cart_full:

    mov ah,9
    lea dx,c12
    int 21h
    jmp customer        


delete_cart:

    mov dl,arr_len
    cmp dl,0
    je cart_empty
    
    
    mov si,0 
    
reset_cart_loop:

    mov dx,0
    mov dl,arr_len
    
    cmp si,dx
    jge reset_done
    mov al,item_arr[si]
    cmp al,1
    je reset_egg
    cmp al,2
    je reset_tomato
    cmp al,3
    je reset_potato
    cmp al,4
    je reset_onion
    cmp al,5
    je reset_garlic
    inc si
    jmp reset_cart_loop

reset_egg:

    mov al,quant_arr[si]    
    
    add egg,al
    jmp increment_si 
    
reset_tomato: 
    mov al,quant_arr[si]
    add tomato,al
    jmp increment_si  
    
reset_potato:

    mov al,quant_arr[si]
    add potato,al
    jmp increment_si 
    
    
reset_onion:       

    mov al,quant_arr[si]
    add onion,al
    jmp increment_si 
    
    
reset_garlic: 
    mov al,quant_arr[si]
    add garlic,al
    jmp increment_si

increment_si:
    inc si
    jmp reset_cart_loop

reset_done:
    mov arr_len,0
    mov ah,9
    lea dx,c9
    int 21h
    jmp choice

place_order:  


    mov ah,9
    lea dx,c10
    int 21h


    cmp arr_len,0
    je cart_empty

    ; Calculate total
    mov si,0
    mov ax,0 
    
    
calculate_total:

    mov dx,0
    mov dl,arr_len
    cmp si,dx  
    
    jge bill_print
    
    mov al,item_arr[si]
    cmp al,1
    je calc_egg
    cmp al,2
    je calc_tomato
    cmp al,3
    je calc_potato
    cmp al,4
    je calc_onion
    cmp al,5
    je calc_garlic

calc_egg:

    mov al,quant_arr[si]
    mov bl,15
    mul bl
    add bill,ax
    jmp next_item

calc_tomato:

    mov al,quant_arr[si]
    mov bl,25
    mul bl
    add bill,ax
    jmp next_item

calc_potato: 

    mov al,quant_arr[si]
    mov bl,20
    mul bl
    add bill,ax
    jmp next_item

calc_onion: 

    mov al,quant_arr[si]
    mov bl,20
    mul bl
    add bill,ax
    jmp next_item

calc_garlic:

    mov al,quant_arr[si]
    mov bl,30
    mul bl
    add bill,ax 
    jmp next_item

next_item:
    inc si
    jmp calculate_total

display_total_bdt: 


    mov divisor1,10000
    mov ten1,10
    
    
    mov ah,9
    lea dx,p
    int 21h
    
    mov ah,9
    lea dx,b1
    int 21h
    ; Print total
    mov ax, 0  
    mov dx, 0 
     
     
    MOV CX, 5       ; 5 Digits output                              
    
    total: 
        mov dx,0      
        mov ax,bill  
        div divisor1     
        mov bill,dx  
                    
        mov dx,ax      
        add dx, 30h     
        mov ah,2
        int 21h 
        

        mov dx,0        
        mov ax,divisor1 
        div ten1         
        mov divisor1,ax   
           
    loop total
    jmp done_shopping 
    
    
display_total_usd:


    mov divisor1,10000
    mov ten1,10

    mov divisor2,10000
    mov ten2,10
    

    
    mov ah,9
    lea dx,p
    int 21h
    
    mov ah,9
    lea dx,b2
    int 21h
    ; Print total
    mov ax, 0  
    mov dx, 0 
     
     
    MOV CX, 5       ; 5 Digits output                              
    
    total1: 
        mov dx,0      
        mov ax,usd  
        div divisor1     
        mov usd,dx  
                    
        mov dx,ax      
        add dx, 30h     
        mov ah,2
        int 21h 
        

        mov dx,0        
        mov ax,divisor1  
        div ten1         
        mov divisor1,ax   
           
    loop total1 
    
    
    mov ah,9
    lea dx,o3
    int 21h
    
    
    MOV CX, 5       ; 5 Digits output                              
    
    total2: 
        mov dx,0      
        mov ax,usd_sents 
        div divisor2     
        mov usd_sents,dx  
                    
        mov dx,ax      
        add dx, 30h     
        mov ah,2
        int 21h 
        

        mov dx,0        
        mov ax,divisor2  
        div ten2        
        mov divisor2,ax   
           
    loop total2
    
    mov ah,9
    lea dx,o4
    int 21h
    
    
    jmp done_shopping 
    
    
    
bill_print:

    mov ah,9
    lea dx,o1
    int 21h
    

    mov ah,9
    lea dx,o2
    int 21h 
    
    mov ah,9
    lea dx,m6
    int 21h 
    
    
    mov ah,1
    int 21h
    sub al,30h
    
    cmp al,1
    je display_total_bdt
    
    cmp al,2
    je conv_usd
    
    mov ah,9
    lea dx,m7
    int 21h
     


conv_usd:  

    mov dx,0

    mov ax,bill
    
    mov bx, 120  ; 120 tk = 1 usd
    
    div bx
    
    mov usd,ax
    mov usd_sents,dx
    
    jmp display_total_usd

    

cart_empty:
    mov ah,9
    lea dx,c8
    int 21h
    jmp choice


not_digit:

    mov ah,9
    lea dx,m7
    int 21h 
    
    mov bl,dig
    
    cmp bl,1
    je change
    
    cmp bl,2
    je add_to_cart
    
    cmp bl,3
    je reset_pas
    
    
done_shopping:


    mov ah,9
    lea dx,o5
    int 21h 

    mov ah,9
    lea dx,o6
    int 21h
     
    mov ah,9
    lea dx,m6
    int 21h  
    
    
    mov ah,1
    int 21h
    sub al,30h
    
    
    cmp al,1
    je wp
    
    cmp al,2
    je exit
    
    mov ah,9
    lea dx,m7
    int 21h 
    
    jmp done_shopping

 

exit:  


    mov ah,9
    lea dx,p
    int 21h
    
    mov ah,9
    lea dx,e1
    int 21h
    






 

;exit to DOS
               
MOV AX,4C00H
INT 21H

MAIN ENDP
    END MAIN
