# meeting for April 2rd

### Meeting Minutes April 2nd

#### Introduction to System Optimization and Program Execution Methods

During the meeting, discussions were held on system design using STM3F103 and GT6 processors. The target processor features one megabyte of flash storage space and 96K of RAM. It is planned to allocate 96K of space for system use, leaving 64K for user programs. Since the system lacks built-in memory and protection modes, both the system and user programs are at the same level and managed by the same scheduler. Specific implementation methods include: firstly, storing user programs in flash, forming executable files through compilation and linking; secondly, allowing code generated on-site to run directly in memory, suitable for non-pre-written codes.

#### Analysis of Memory Management and Task Scheduling Strategies

The components of tasks including code, data, and context were clarified first, with data possibly residing in either flash or memory. During scheduling, the system can unconditionally transfer control from the current thread to other tasks or automatically reclaim idle threads and switch to new tasks via a timer. Moreover, threads have different priorities, with higher priority threads being executed preferentially. Finally, four thread states were introduced: suspended, sleeping, normal scheduling, and real-time mode, where the former three are conventional multitasking processes, while real-time mode ensures certain tasks with strict timing requirements are not interrupted.

#### Discussion on Thread Management and Memory Swapping Strategies

Discussion focused on operations such as swapping threads in and out of memory, usage and limitations of external storage, and how to achieve direct memory address mapping through FSMC. To address efficiency issues caused by frequent swapping in and out of threads, a strategy was proposed to swap out threads that haven't been scheduled after ten attempts. Additionally, the realization of virtual memory in microcontrollers was discussed, which uses stacks to ensure each thread can only access its own memory area and prohibits the creation of global variables by threads. Lastly, methods for implementing shared variables were discussed, with the system providing addresses of shared variables through APIs and automatically reclaiming allocated memory upon thread destruction to prevent memory leaks.

#### Discussion on Thread Scheduling and Memory Area Management Strategies

Discussion centered around setting up areas for thread execution, emphasizing that local variables contain addresses, hence different threads cannot operate within the same area. Three different thread scheduling models were discussed: the first model involves independent thread scheduling for each area but requires attention to pointer address correspondence; the second model involves all threads sharing a 64K space formed by mapping external memory, which is efficient but requires frequent data exchanges; the third model involves setting thread scheduling for different areas based on priority, which can improve efficiency but requires users to specify priorities. Preemptive and non-preemptive scheduling were also discussed.

#### Optimization Strategies for Memory Management and Pointer Operations

For issues related to memory allocation and pointer operations, a fourth mode was proposed. In this mode, the system automatically allocates required memory and adjusts pointer operations. However, this method may reduce memory access speed since each memory access requires adding a specific amount. Furthermore, intermediate mapping processes may consume considerable time, especially during high-frequency operations like copying data, leading to reduced overall efficiency.

#### Implementation of Multitasking Switching Under Hardware Mode

Discussions revolved around hardware usage and management, including utilizing peripheral spaces within hardware, maintaining structures within the system's 64K space, and employing external memory for users. The characteristics of SMC were introduced, which exposes itself only to the chip and accesses only 16 address lines, thereby enabling multitasking switching. Detailed explanations were provided on how to select half of a large memory space by changing three address lines. Emphasis was placed on the importance of the system's memory address management unit, indicating it meets the requirements of internal management. A new proposal was discussed, which requires no additional material costs but involves handing over all user programs to the system and creating interfaces within the system.

#### System Scheduling and Strategy for Transferring Control Rights Between Threads

Discussion included system calls, real-time processing, and three ways of transferring control rights from user programs back to the system. All system calls depend on an interrupt occurring every millisecond. When an interrupt occurs, the system processes all tasks within a short period before returning to the user scene. There are three ways for the system to gain control, including the terminal system gaining initial control, special millisecond interrupts, and explicit function calls by users. The system should avoid interruptions during operation but must respond to more urgent external interrupts when they occur. A watchdog timer mechanism was also introduced.

#### Program Execution and State Tracking in Multi-threaded Programming

Discussions covered issues concerning thread switching and function calls during program execution. For scenarios involving multiple nested functions, strategies for storing and restoring the execution position of threads were proposed. One approach is to use the program counter (PC) for saving the execution position, with the system automatically saving and restoring the PC during interrupt handling. Additionally, detailed explanations were provided on a complete scheduling process.

#### Discussion on Thread Return Handling and Memory Management Strategies

Discussion involved how the system handles thread returns and manages memory. Firstly, when a thread returns, it goes back to the system, which performs cleanup work, including releasing the memory resources occupied by the thread. Secondly, users can request the system to erase data after a thread returns by setting system flags, rather than simply zeroing out the entire memory region allocated to it. Moreover, a thread with the lowest priority could be established for cleaning unused memory areas when no other threads need to be scheduled.

#### Discussion on Internal and External Memory Management and Usage of Global Variables

Discussion mainly focused on the implementation of internal 4032 memory and external memory, explaining why development boards purchased by users have a reserved spot for welding chips on their backsides. Emphasis was placed on the principle that threads handle their own matters, and user programs only need to ensure they can access necessary areas. The risks associated with the use of global variables were discussed, advocating against their use to avoid problems arising from data inconsistency.

#### Discussion on System Synchronization and Multi-thread Management Strategies

Discussion encompassed working modes under single-core and multi-core environments and how to implement synchronization locks and semaphores for synchronized multithreading. Proposals for resource protection using the interrupt system for peripherals were made. Additionally, the significance of network concepts in simulations was highlighted, along with exploring ways to establish connections between chips.

#### Discussion on Inter-chip Communication and Single Chip Optimization Strategies

Discussion involved communication among multiple chips and the implementation process of designing robotic arms on a single chip. Despite the current limited practical application of communication among multiple chips, potential improvements were left open for future developments. Designs should ensure normal inter-chip communication before considering how to utilize external memory and Harvard architecture for inter-chip access. Due to limited hardware resources, designs needed optimization, such as reducing the number of signal lines and considering exclusive memory on a single chip.

#### Optimization of Operating System Resource Management and Communication Efficiency

Discussion centered around the operating method of the OS and its role in multitasking processing. Mention was made of strategies such as sacrificing a shell to adjust plates and fit pipes to predict and control the OS. It was emphasized that other technologies would be paused during OS operation, whereas dual-core systems could simultaneously run multiple tasks. Discussions also touched upon potential efficiency issues that might arise from having all operations handled by the OS and methods and efficiency of information transmission during communication.
