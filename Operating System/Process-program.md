Bilkul. **Program → Code → Process → Process Table → Process States** ko ek chain ki tarah samjho. Ye OS ka bahut fundamental topic hai, aur yahin par usually confusion hota hai ki **"code ko process bolte hain kya?"** — answer hai **nahi**.

# 1. Sabse pehle: Program kya hota hai?

Simple definition:

> **Program instructions ka collection hota hai jo computer ko batata hai ki kya kaam karna hai.**

Example:

```c
#include <stdio.h>

int main()
{
    int a = 10;
    int b = 20;
    int sum = a + b;

    printf("%d", sum);

    return 0;
}
```

Ye ek **program** hai.

Is program ka purpose hai:

```text
10 + 20
  ↓
30
```

Program basically instructions ka set hai.

---

# 2. Code kya hota hai?

**Code** wo instructions/statements hote hain jo programmer programming language mein likhta hai.

Example:

```c
int a = 10;
int b = 20;
int sum = a + b;
```

Ye code hai.

Multiple code statements milkar program bana sakte hain.

```text
Code
 ├── Statement 1
 ├── Statement 2
 ├── Statement 3
 └── Statement 4
       ↓
    Program
```

Lekin "code" aur "program" everyday programming language mein kabhi-kabhi interchangeably use hote hain. OS ke context mein humein distinction samajhna important hai.

---

# 3. Program aur Code mein difference

Example:

```c
int a = 10;
int b = 20;
int sum = a + b;
printf("%d", sum);
```

Ye **source code** hai.

Agar is code ko compile kiya:

```text
Source Code
     ↓
Compiler
     ↓
Machine Code / Executable
     ↓
Program executable
```

For example Windows par:

```text
calculator.exe
```

ek executable program ho sakta hai.

---

# 4. Important: Program aur Process same nahi hain

Ye line notes mein bold karke likh sakte ho:

> **Program is a passive entity; Process is an active instance of a program in execution.**

Simple Hinglish:

> **Program = kya karna hai, uski instructions.**
>
> **Process = un instructions ko actually execute karne wala running instance.**

Example:

```text
Program:
Chrome.exe
```

Jab tum Chrome start karte ho:

```text
Chrome.exe
     ↓
OS loads it into memory
     ↓
Process created
     ↓
CPU executes it
```

So:

```text
PROGRAM
   ↓
Execution ke liye start
   ↓
PROCESS
```

---

# 5. Real-life example — Recipe

Ek bahut simple analogy:

### Recipe = Program

Suppose recipe hai:

```text
1. Water boil karo
2. Tea powder add karo
3. Milk add karo
4. Sugar add karo
5. Tea serve karo
```

Ye **instructions** hain.

### Actual cooking = Process

Jab tum recipe follow karke actually chai bana rahe ho:

```text
Recipe
  ↓
Actual execution
  ↓
Cooking Process
```

So:

```text
Program = Instructions
Process = Instructions being executed
```

---

# 6. Ek aur example — `.exe`

Suppose tumhare paas:

```text
MyApp.exe
```

hai.

Ye disk/SSD par stored executable program hai.

Ab tum double-click karte ho:

```text
MyApp.exe
    ↓
OS
    ↓
Program ko memory mein load karta hai
    ↓
Process create hota hai
    ↓
CPU execution start karta hai
```

Yahan:

```text
MyApp.exe = Program / executable image
Running MyApp = Process
```

---

# 7. Kya code ko Process bolte hain?

**Nahi.**

Ye distinction bahut important hai.

```text
Source Code
     ↓
Compiler
     ↓
Executable Program
     ↓
OS loads it
     ↓
Process
```

Example:

```c
printf("Hello");
```

Ye **code** hai.

Compiled application:

```text
hello.exe
```

Ye **executable program** hai.

Jab run karoge:

```text
hello.exe
    ↓
Process
```

---

# 8. Kya ek Program ke multiple Processes ho sakte hain?

**Yes.**

Ye bahut important concept hai.

Suppose:

```text
MyApp.exe
```

ek program hai.

Agar tum usko multiple times run karte ho:

```text
MyApp.exe
   ↓
Process 1

MyApp.exe
   ↓
Process 2

MyApp.exe
   ↓
Process 3
```

Same program ki multiple **process instances** ho sakti hain.

Har process ki apni execution state hoti hai.

---

# 9. Chrome example

Browser ko example lete hain.

Conceptually:

```text
Chrome Program
      ↓
   Execution
      ↓
Process(es)
```

Modern browsers often use multiple processes for isolation and other reasons.

For example conceptually:

```text
Chrome
 ├── Browser Process
 ├── Renderer Process
 ├── GPU Process
 └── Other supporting processes
```

Exact architecture browser/version par depend karti hai.

Important point:

> **Ek application ka matlab zaroori nahi ki exactly ek process ho.**

---

# 10. Process exactly kya hota hai?

Ab proper definition:

> **A process is a program in execution, along with its current execution state and the resources associated with that execution.**

Hinglish:

> **Process ek running program ki instance hoti hai, jiske saath uski current state, memory, CPU-related information, resources, etc. associated hote hain.**

Isliye process sirf code nahi hai.

Process ke andar/saath bahut kuch hota hai.

Conceptually:

```text
             PROCESS
                │
     ┌──────────┼──────────┐
     ↓          ↓          ↓
   Code       Data       Stack
     │
     ├── Heap
     ├── CPU State
     ├── Registers
     ├── Program Counter
     ├── Resources
     └── OS Management Info
```

---

# 11. Process ke andar kya hota hai?

Ek running process ko roughly memory aur execution information ke form mein samjho.

Example:

```text
Process
│
├── Code / Text Section
├── Data Section
├── Heap
├── Stack
├── CPU Registers
├── Program Counter
└── OS-managed resources
```

Ab one-by-one.

---

# 12. Code / Text Section

Program ke executable instructions.

Example:

```c
int sum(int a, int b)
{
    return a + b;
}
```

Iska compiled machine-level representation executable memory mein instructions ke form mein hota hai.

Conceptually:

```text
Process
└── Code
     ├── Instruction 1
     ├── Instruction 2
     ├── Instruction 3
     └── Instruction 4
```

---

# 13. Data Section

Global/static variables jaise data ke liye memory areas hoti hain.

Example:

```c
int globalCounter = 10;
```

Ye global variable hai.

Conceptually:

```text
Process
└── Data
     └── globalCounter = 10
```

---

# 14. Stack

Function calls aur local variables ke liye stack memory use hoti hai.

Example:

```c
void test()
{
    int x = 10;
    int y = 20;
}
```

`x` aur `y` jaise local variables typically stack par hote hain (language/compiler/runtime details ke according).

Function call:

```text
main()
  ↓
test()
  ↓
return
```

Stack function call information maintain karne mein important hai.

---

# 15. Heap

Dynamic memory allocation ke liye heap use hota hai.

Example C:

```c
int *p = malloc(sizeof(int));
```

Yahan dynamically allocated memory heap se aa sakti hai.

C#/.NET mein:

```csharp
var user = new User();
```

`new` ke through created objects generally managed heap par allocate hote hain.

OS/process memory model ke perspective se heap ek important region hai, although exact runtime behavior language/runtime par depend karta hai.

---

# 16. Program Counter

Ab OS/process ka bahut important part.

**Program Counter (PC)** CPU ko batata hai ki next instruction kahan se execute karni hai.

Conceptually:

```text
Code:

Instruction 1
Instruction 2
Instruction 3
Instruction 4
Instruction 5
```

Agar:

```text
PC → Instruction 3
```

to CPU next execution ke liye instruction 3 ko fetch karega.

Execution ke baad PC update hota hai.

```text
PC
 ↓
Instruction
 ↓
Execute
 ↓
PC updated
 ↓
Next Instruction
```

---

# 17. CPU Registers

CPU ke andar small, very-fast storage locations hoti hain jinko registers kehte hain.

Process execution ke context mein important CPU state mein registers bhi aate hain.

Example:

```text
General Purpose Registers
Program Counter
Stack Pointer
Status/Flags
```

Architecture ke according exact registers different hote hain.

---

# 18. Process ke resources

Process ko resources bhi chahiye ho sakte hain.

For example:

```text
Process
 │
 ├── CPU
 ├── Memory
 ├── Files
 ├── Network sockets
 └── Devices
```

OS in resources ko manage/control karta hai.

---

# 19. Ab Process Table kya hoti hai?

Ab hum OS ke important concept par aate hain:

> **Process Table OS ki data structure/table hoti hai jisme active processes ko manage karne ke liye important information maintain ki jati hai.**

Simple:

```text
                 OPERATING SYSTEM
                       │
                 Process Table
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
    Process 1      Process 2      Process 3
```

Conceptually process table mein har process ke liye information hoti hai.

---

# 20. Process Table mein kya hota hai?

Exact implementation OS ke according different hoti hai.

Generally process-related information include ho sakti hai:

```text
PID
Process State
Program Counter
CPU Registers / saved CPU context
Scheduling Information
Memory Management Information
Accounting Information
I/O / Open File Information
Security / Credentials
Parent Process Information
```

Is information ko manage karne ke liye OS commonly **PCB — Process Control Block** use karta hai.

---

# 21. PCB kya hota hai?

**PCB = Process Control Block**

> PCB ek OS data structure hai jo ek particular process ko manage karne ke liye required information store karta hai.

Conceptually:

```text
Process Table
│
├── PCB → Process 1
├── PCB → Process 2
├── PCB → Process 3
└── PCB → Process 4
```

Ya:

```text
             PROCESS TABLE
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
      PCB        PCB        PCB
       ↓          ↓          ↓
   Process 1  Process 2  Process 3
```

---

# 22. PCB mein kya-kya information ho sakti hai?

Example:

```text
PCB
│
├── PID
├── Process State
├── Program Counter
├── CPU Registers
├── CPU Scheduling Information
├── Memory Management Information
├── Accounting Information
├── I/O Status Information
└── Open File Information
```

Ab important fields samjho.

---

# 23. PID — Process ID

Har process ko OS ek identifier de sakta hai.

Example:

```text
PID 1001 → Chrome
PID 1050 → VS Code
PID 1100 → Spotify
```

Linux mein tum processes dekhne ke liye commands jaise:

```bash
ps
```

ya

```bash
top
```

use kar sakte ho.

Windows mein Task Manager process information show karta hai.

---

# 24. Process State

PCB mein process ki current state maintain hoti hai.

Example:

```text
Process A
State = Running
```

Ya:

```text
Process B
State = Waiting
```

---

# 25. Program Counter in PCB

Suppose process execute kar raha tha:

```text
Instruction 100
Instruction 101
Instruction 102
Instruction 103
```

Ab CPU ko process switch karna pada.

OS ko process ki execution position preserve karni hogi.

For example:

```text
Process A
PC = Instruction 103
```

Ye information PCB/context mein save ho sakti hai.

Baad mein Process A resume hoga:

```text
PCB
 ↓
Saved CPU Context
 ↓
PC restored
 ↓
Execution continues
```

Isi concept ka connection **context switching** se hai.

---

# 26. Process States kya hoti hain?

Process hamesha CPU par execute nahi hota.

Ek process apni lifetime mein different states mein ja sakta hai.

Basic model:

```text
NEW
 ↓
READY
 ↓
RUNNING
 ↓
TERMINATED
```

Aur I/O waiting ki wajah se:

```text
RUNNING
   ↓
 WAITING
   ↓
 READY
```

Ab complete flow samjho.

---

# 27. NEW State

Jab process create ho raha hota hai:

```text
Program
   ↓
OS creates process
   ↓
NEW
```

Example:

Tum:

```text
calculator.exe
```

open karte ho.

OS process create kar raha hai.

Conceptually:

```text
NEW
```

state.

---

# 28. READY State

Process ready hai CPU par run hone ke liye, lekin CPU abhi kisi aur process ko mila hua hai.

```text
Process A
State = READY
```

Meaning:

> "Main execute karne ke liye ready hoon, mujhe CPU milte hi main run kar sakta hoon."

Ready processes ko scheduler manage karta hai.

Conceptually:

```text
READY QUEUE

Process A
Process B
Process C
     │
     ↓
   Scheduler
     │
     ↓
    CPU
```

---

# 29. RUNNING State

Jab process ko CPU mil jata hai:

```text
READY
  ↓
Scheduler selects
  ↓
RUNNING
```

Ab CPU process ki instructions execute kar raha hai.

Example:

```text
Process A
   ↓
CPU
   ↓
Instruction execution
```

---

# 30. WAITING / BLOCKED State

Suppose process ko disk se data chahiye.

Process CPU par execute kar raha tha:

```text
RUNNING
   ↓
"I need data from disk"
   ↓
WAITING / BLOCKED
```

Process ab CPU ko use karke continuously wait karne ki zarurat nahi rakhta.

OS kisi doosre ready process ko CPU de sakta hai.

```text
Process A → Waiting for Disk

Process B → CPU
```

Jab disk operation complete:

```text
Disk complete
     ↓
Process A
     ↓
READY
```

Phir scheduler later CPU de sakta hai.

---

# 31. TERMINATED State

Jab process execution complete ho jata hai:

```text
RUNNING
   ↓
Exit
   ↓
TERMINATED
```

OS process ke resources clean up/reclaim karta hai according to OS semantics.

---

# 32. Complete Process State Diagram

Ye tumhare notes mein zaroor hona chahiye:

```text
                    ┌──────────┐
                    │   NEW    │
                    └────┬─────┘
                         │
                         ↓
                    ┌──────────┐
              ┌────→│  READY   │←──────────┐
              │     └────┬─────┘           │
              │          │                 │
              │          │ Scheduler       │
              │          ↓                 │
              │     ┌──────────┐           │
              │     │ RUNNING  │           │
              │     └────┬─────┘           │
              │          │                 │
              │     ┌────┴───────┐         │
              │     │            │         │
              │     ↓            ↓         │
              │  I/O wait     Exit         │
              │     ↓            ↓         │
              │ ┌─────────┐  ┌──────────┐   │
              └─│ WAITING │  │TERMINATED│   │
                └────┬────┘  └──────────┘   │
                     │                      │
                  I/O done                  │
                     │                      │
                     └──────────────────────┘
```

---

# 33. Process READY se RUNNING kaise hota hai?

Suppose:

```text
Process A → READY
Process B → READY
Process C → READY
```

CPU free/available scheduling decision hota hai.

Scheduler:

```text
Ready Queue
    ↓
CPU Scheduler
    ↓
Select Process A
    ↓
RUNNING
```

So:

```text
READY → RUNNING
```

---

# 34. RUNNING se WAITING kaise?

Suppose process ko file read karni hai.

```text
RUNNING
   ↓
File read request
   ↓
I/O not completed
   ↓
WAITING
```

CPU kisi aur ready process ko mil sakta hai.

---

# 35. WAITING se READY kaise?

Disk ne data de diya:

```text
WAITING
   ↓
I/O complete
   ↓
READY
```

Ab process CPU ke liye ready hai.

---

# 36. RUNNING se READY kaise?

Important case.

Suppose process ko CPU mil gaya:

```text
Process A
   ↓
RUNNING
```

Lekin scheduler kisi aur process ko CPU dena chahta hai.

For example time slice expire:

```text
RUNNING
   ↓
Preemption
   ↓
READY
```

Isko CPU scheduling ke context mein samjhenge.

---

# 37. RUNNING se TERMINATED

Process complete:

```text
RUNNING
   ↓
exit()
   ↓
TERMINATED
```

Ya process ko terminate kiya ja sakta hai.

---

# 38. Ek real example — Calculator

Suppose tum calculator application open karte ho.

### Step 1

Executable/program available:

```text
calculator.exe
```

### Step 2

Tum double-click karte ho.

OS process create karta hai.

```text
Program
  ↓
Process Created
```

### Step 3

Process initially:

```text
NEW
```

### Step 4

OS usko runnable banata hai:

```text
READY
```

### Step 5

Scheduler CPU deta hai:

```text
RUNNING
```

### Step 6

Calculator input wait kar raha hai:

```text
WAITING
```

### Step 7

Tum `10 + 20` type karte ho.

Input/event available:

```text
WAITING
   ↓
READY
```

### Step 8

Scheduler CPU deta hai:

```text
READY
   ↓
RUNNING
```

### Step 9

Calculation hoti hai.

```text
10 + 20
   ↓
30
```

### Step 10

Calculator close:

```text
RUNNING
   ↓
TERMINATED
```

Ye process lifecycle ko samajhne ka simple example hai.

---

# 39. Program vs Process — final comparison

| Program                                      | Process                                                   |
| -------------------------------------------- | --------------------------------------------------------- |
| Instructions ka set                          | Running/executing instance                                |
| Passive entity                               | Active entity                                             |
| Usually disk par executable form mein stored | Execution ke time OS ke under managed                     |
| Khud execute nahi kar raha                   | CPU par execute ho raha / runnable / waiting ho sakta hai |
| Code/instructions represent karta hai        | Code + execution state + resources                        |
| Multiple processes create kar sakta hai      | Har instance ki separate execution state ho sakti hai     |

Sabse important:

```text
PROGRAM
   ↓
Execution
   ↓
PROCESS
```

---

# 40. Source Code → Program → Process

Is complete chain ko yaad rakho:

```text
                PROGRAMMING
                     │
                     ↓
              Source Code
                     │
                     ↓
          Compiler / Build Process
                     │
                     ↓
             Executable Program
                     │
              User runs it
                     ↓
               Operating System
                     │
                     ↓
                New Process
                     │
                     ↓
            Process enters READY
                     │
                     ↓
                Scheduler
                     │
                     ↓
                 RUNNING
                     │
            ┌────────┴────────┐
            ↓                 ↓
         WAITING          READY again
            │                 │
            └──────→──────────┘
                     │
                     ↓
                TERMINATED
```

---

# 41. "Line of Code" kya hoti hai?

Ab tumhare question ka ye part bhi important hai.

Suppose:

```c
int a = 10;
int b = 20;
int sum = a + b;

printf("%d", sum);
```

Yahan visually 4 statements hain.

Lekin **line of code** aur **instruction executed by CPU** same thing nahi hain.

Ye bahut important distinction hai.

---

# 42. Source-code line ≠ CPU instruction

Tum likhte ho:

```c
int sum = a + b;
```

Ye source-code statement hai.

Compiler isko multiple machine instructions mein convert kar sakta hai.

Conceptually:

```text
Source Code
int sum = a + b;
       ↓
Compiler
       ↓
Machine Instructions

LOAD a
LOAD b
ADD
STORE sum
```

Exact generated instructions CPU architecture, compiler, optimization level etc. par depend karte hain.

So:

> **Ek line of source code necessarily ek CPU instruction nahi hoti.**

---

# 43. CPU kya execute karta hai?

CPU ultimately machine-level instructions execute karta hai.

Example conceptual:

```text
Source Code:

int c = a + b;
```

↓

Compiler

```text
Machine Code / Instructions
```

↓

CPU:

```text
Fetch
Decode
Execute
```

So complete chain:

```text
Human
 ↓
Source Code
 ↓
Compiler
 ↓
Machine Instructions
 ↓
Process Memory
 ↓
CPU
 ↓
Execution
```

---

# 44. Process mein source code hota hai kya?

Directly source code nahi.

Suppose:

```c
printf("Hello");
```

Tumhara `.c` file source code hai.

Compile hone ke baad executable/machine-code representation banti hai.

Run hone par OS executable ko process ke address space mein load/manage karta hai.

Conceptually:

```text
hello.c
  ↓
Compiler
  ↓
hello.exe
  ↓
OS loads
  ↓
Process
  ↓
CPU executes machine instructions
```

---

# 45. Process sirf Code nahi hai

Ye exam/interview ke liye **very important point** hai.

Wrong:

```text
Process = Code
```

Correct:

```text
Process =
    Program Code
  + Data
  + Stack
  + Heap
  + CPU State
  + Program Counter
  + Resources
  + OS Management Information
```

Exact memory layout and OS implementation details vary, but conceptually ye model useful hai.

---

# 46. Process Table + PCB ko ek saath samjho

Suppose system mein 3 processes hain:

```text
Chrome
VS Code
Spotify
```

OS internally process-management information maintain karta hai.

Conceptually:

```text
                 PROCESS TABLE
┌─────────────────────────────────────┐
│ PID   State      Program            │
├─────────────────────────────────────┤
│ 101   Running    Chrome             │
│ 205   Ready      VS Code            │
│ 310   Waiting    Spotify            │
└─────────────────────────────────────┘
```

Behind each process, PCB-style information can contain much more:

```text
PCB for Chrome
├── PID
├── State
├── Program Counter
├── Registers/context
├── Scheduling info
├── Memory info
├── I/O info
└── File/resource info
```

---

# 47. Context Switching se connection

Ab maan lo:

```text
Process A → RUNNING
```

OS ko Process B run karna hai.

OS ko A ki current execution state save karni padegi:

```text
Process A
   ↓
Save CPU Context
   ↓
PCB / process context
```

Then B ki saved state load:

```text
Process B
   ↓
Restore CPU Context
   ↓
CPU
```

Then:

```text
Process A
    ↓
Process B
```

Ye **context switch** hai.

Aur isi liye PCB ka concept important hai.

---

# 48. Sab concepts ko ek picture mein dekho

```text
                 SOURCE CODE
                      │
                      ↓
                Compiler
                      │
                      ↓
             EXECUTABLE PROGRAM
                      │
                User runs it
                      │
                      ↓
               Operating System
                      │
                      ↓
                PROCESS CREATED
                      │
         ┌────────────┼────────────┐
         │            │            │
       Code          Data       Resources
         │            │            │
         ├────────────┼────────────┤
         │
       Stack
       Heap
       PC
       Registers
         │
         ↓
    PROCESS CONTROL BLOCK
         │
         ↓
    PROCESS TABLE
         │
         ↓
   ┌─────────────────────┐
   │ Process State        │
   │ NEW                  │
   │ READY                │
   │ RUNNING              │
   │ WAITING/BLOCKED      │
   │ TERMINATED           │
   └─────────────────────┘
         │
         ↓
   CPU SCHEDULER
         │
         ↓
        CPU
         │
         ↓
   MACHINE INSTRUCTIONS
```

---

# 49. Ek line mein har term

| Term                | Simple Meaning                                                            |
| ------------------- | ------------------------------------------------------------------------- |
| **Code**            | Programmer ke written instructions                                        |
| **Source Code**     | Human-readable programming language mein code                             |
| **Program**         | Instructions ka complete executable concept/set                           |
| **Executable**      | Run hone layak compiled/built program                                     |
| **Process**         | Program ki currently executing instance                                   |
| **PCB**             | Ek process ko manage karne ki OS information                              |
| **Process Table**   | Multiple processes ki management information ka OS-level collection/table |
| **PID**             | Process ka identifier                                                     |
| **Process State**   | Process abhi kis stage mein hai                                           |
| **Program Counter** | Next instruction ki execution location                                    |
| **Stack**           | Function calls/local execution data ke liye memory area                   |
| **Heap**            | Dynamic memory ke liye memory area                                        |
| **Scheduler**       | Decide karta hai runnable processes/threads mein kisko CPU time mile      |
| **Context Switch**  | Ek execution context ko save karke doosre ko restore karna                |

---

# 50. Sabse important mental model

Agar tum OS properly samajhna chahte ho, to is chain ko mind mein rakho:

```text
CODE
 ↓
PROGRAM
 ↓
PROGRAM IS RUN
 ↓
PROCESS
 ↓
PROCESS HAS STATE
 ↓
OS MANAGES PROCESS
 ↓
PCB STORES PROCESS INFORMATION
 ↓
PROCESS TABLE TRACKS PROCESSES
 ↓
SCHEDULER SELECTS RUNNABLE WORK
 ↓
CPU EXECUTES INSTRUCTIONS
```

Aur process lifecycle:

```text
        NEW
         ↓
       READY
         ↓
      RUNNING
       ↙    ↘
  WAITING   READY
     ↓
   READY

RUNNING
   ↓
TERMINATED
```

**Next logical topic:** ab iske baad **Thread kya hota hai, Process vs Thread mein exact difference kya hai, ek process ke andar threads kaise hote hain, aur CPU actually process/thread ko kaise execute karta hai** — ye samajhna natural next step hai.
