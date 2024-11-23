# Demonstrate Access Contention Problems in a Multitasking System

This project demonstrates access contention problems that arise when using shared resources in a multitasking environment. The program is designed for an STM32 microcontroller running FreeRTOS, where multiple threads attempt to access a shared resource without proper synchronization mechanisms. A Blue LED is used as an indicator of resource contention or collisions.

## Project Description

In this program:
1. **Threads**:
   - **RelLedFlashing Task**: Toggles a Red LED and attempts to access the shared Blue LED resource.
   - **GreenLedFlass Task**: Toggles a Green LED and also attempts to access the shared Blue LED resource.
   
2. **Access Contention**:
   - Both the `RelLedFlashing` and `GreenLedFlass` tasks call the `accessData()` function to	hold	data	that	is	used	by	both	tasks	(a	datastore). In	general,	data	can	be	written	to	and	read	from	the	store
   - it's	important	to	understand	that	the	'whens'	of	read	and	write operations	are	dictated	by	the	individual	tasks;	there	is	no	overall	control.	Thus there	is	always	a	possibility	that	individual	tasks	may	simultaneously	write	to	the store,	for	example;	or	one	may	be	writing	while	the	other	is	reading.	This contention	for	resources	can	result	in	data	being	corrupted,	leading	(possibly)	to task	misbehaviour. True	simultaneous	access	can	take	place	only	in	multicomputer/multicore systems;	it	cannot	occur	in	single	processor	designs.	With	these	it	is	a	form	of 'apparent'	simultaneous	access;	unfortunately	this	can	still	lead	to	corrupted	data being	used	by	the	tasks.

3. **Shared Resource**:
   - The Blue LED is shared between the tasks. Contention issues occur because access to this GPIO pin is not protected by any locking mechanism.

4. **Objective**:
   - To demonstrate the need for synchronization in multitasking systems.
   - To show how introducing proper synchronization (e.g., mutexes or `taskENTER_CRITICAL` and `taskEXIT_CRITICAL`) can resolve such issues.

---
### Simulated Critical Operation

The `accessData()` function simulates a critical read/write operation in a multitasking system. In a real-world scenario, such operations might involve manipulating shared variables, peripherals, or hardware resources that require atomic access to maintain system integrity. 

Without proper synchronization, simultaneous access by multiple tasks can cause unpredictable behavior or data corruption. In this program, the Blue LED acts as a collision indicator:
- If a task is performing a critical operation and is interrupted by another task accessing the same resource, the Blue LED turns ON, signaling a collision.

### Tasks and Resource Contention

1. **Tasks**:
   - **RelLedFlashing Task**:
     - Toggles a Red LED.
     - Calls `accessData()` to simulate a critical operation.
   - **GreenLedFlass Task**:
     - Toggles a Green LED.
     - Also calls `accessData()` to simulate access to the same shared resource.
   
2. **Shared Resource**:
   - Both `RelLedFlashing` and `GreenLedFlass` tasks attempt to modify the Blue LED state, representing access to a shared resource.

3. **Resource Contention**:
   - When one task is inside the `accessData()` function, another task can interrupt and access the same function. This causes a "collision," indicated by the Blue LED toggling.

### Objective
The main goal is to demonstrate:
- The importance of synchronizing access to shared resources in multitasking systems.
- How synchronization mechanisms (e.g., semaphores, mutexes, or critical sections) prevent collisions and ensure data integrity.

---

## Hardware Requirements

- **Microcontroller**: STM32 (configured with FreeRTOS)
- **Peripherals**:
  - **Blue LED**: Indicates resource contention (collisions).
  - **Red LED**: Controlled by the `RelLedFlashing` task.
  - **Green LED**: Controlled by the `GreenLedFlass` task.
- **Other Components**:
  - Power supply for the STM32 board
  - Debugging tools (e.g., ST-Link)

---

## Code Explanation

### Key Functions

- **`accessData()`**:
  - Simulates a critical section where a shared resource is accessed.
  - Introduces a delay (`HAL_Delay`) to represent a long operation.
  - Checks if another task interrupts the current operation with check the flag state in `accessdata` funtion. If flag is false or indicated the previous task was interrupted, the Blue LED toggles to indicate a collision.

- **Task Functions**:
  - `RelLedFlashing`: Toggles the Red LED and calls `accessData()`.
  - `GreenLedFlass`: Toggles the Green LED and calls `accessData()`.

- **Collision Behavior**:
  - If `accessData()` is interrupted by another task, the shared resource (Blue LED) enters an unintended state, visualized by the LED turning ON.

---

## Steps to Demonstrate

1. **Without Synchronization**:
   - Flash the provided code onto the STM32 microcontroller.
   - Observe the behavior of the Blue LED. It will toggle ON when a collision occurs due to multiple tasks accessing the shared resource simultaneously.

2. **With Synchronization** (to be implemented in the next task):
   - Add a critical section (`taskENTER_CRITICAL`/`taskEXIT_CRITICAL`).
   - Re-flash the program and observe the absence of collisions (Blue LED no longer toggles ON).

---
## To-Do for Final Report

- [ ] **Photos of the Hardware Setup**:
  ![IMG-20241123-WA0007 1](https://github.com/user-attachments/assets/9ba7813b-3ae6-42d9-a5c1-53de16ab18e0)


- [ ] **Video of the Experiment**:


https://github.com/user-attachments/assets/3db54d25-7f04-4d90-8fbd-869e25e5f3cb



---
