### **Assignment 1, Module 2: Auditing and Test Cases**

This assignment focuses on understanding and improving the security of a C program (`giftcardreader.c`) and its accompanying header file (`giftcard.h`). You will audit the program, identify flaws, and create test cases to expose them. Additionally, you'll remediate the identified issues and implement regression tests to ensure the fixes persist.

---

### **Step 1: Understand the Program**

1. **Explore the Code**
   - Read through `giftcardreader.c` and `giftcard.h` to understand how they work.
   - Build and run the program with the included `examplefile.gft` to observe its normal behavior.
   - Use a debugger such as **GDB** or **LLDB** to step through the program during execution. This helps uncover logic flaws or unexpected behaviors.
     Feel free to use whatever debugger you prefer, but we strongly recommend using one so that you can more easily trace your work.
     - **Definition**: A *debugger* is a tool that allows you to execute a program line by line, inspect variable values, and monitor program execution to identify bugs.

2. **Diagram the Flow**
   - Create a diagram or write a description showing how different parts of the code interact. This is not to submit, but part of a recommend process in understanding any type of codebase!

3. **Using the Makefile**
   - The provided **Makefile** simplifies the build process:

     - **Run `make`**: Compiles the program.
     - **Run `make test`**: Executes a minimal test suite using `runtests.sh`. This is a very minimal test suite, you should create additional tests! This script runs the gift card reader on files in `testcases/valid` and `testcases/invalid`.
       - **Valid test case**: A test that works as expected.
       - **Invalid test case**: A test that triggers unexpected behavior from the application.

4. **Generated Binaries**
   - The Makefile compiles three versions of the program:
     - **`giftcardreader.original`**: Unmodified version of the program.
     - **`giftcardreader.asan`**: Uses AddressSanitizer, a tool for detecting memory-related errors.
       - **Definition**: AddressSanitizer flags memory issues like buffer overflows or use-after-free errors.
     - **`giftcardreader.ubsan`**: Uses UndefinedBehaviorSanitizer, which detects undefined behavior (e.g., null pointer dereferencing, integer overflows).
       - **Definition**: UndefinedBehaviorSanitizer helps identify actions that lead to unpredictable behavior in C/C++ programs.

---

### **Step 2: Find and Document Flaws**

1. **Identify Flaws**
   - Audit the program to uncover issues using the provided binaries (`asan`, `ubsan`, `original`) and tools (such as a debugger).
   - Use test cases to identify crashes or unexpected behavior. Employ debugging tools to trace the root causes of issues. Remember, you can go ahead and create giftcards of your own as an input here!

2. **Create Test Cases**
   - Write the following test files to expose flaws in the program (label them exactly as below):
     - **`crash1.gft`**: Triggers a crash with a unique root cause.
     - **`crash2.gft`**: Triggers a crash with a different root cause.
     - **`hang.gft`**: Causes the program to loop indefinitely. (Hint: Investigate the "animation" record type for bugs.)
    
     Think carefully about where to store them, see Step 1.3.

3. **Understand Crash Indicators**
   - A crash typically results in a **Segmentation Fault** or other fatal error. After running the program, check the exit code using the `echo $?` command:
     - **0**: Indicates successful execution.
     - **Non-zero**: Indicates an error occurred.

4. **Document Findings**
   - Create a text file (`part2.txt`) explaining the bugs triggered by each of your test cases:
     - Root cause of the bug.
     - Behavior observed during the crash/hang.
     - Steps to reproduce the issue.
     - Explain, clearly, why it works, what lines it addreses, how you found the error. Not knowing how to explain your findings will result in you having a very unpleasant time during your Assesment Quiz!

---

### **Step 3: Fix the Bugs**

1. **Implement Fixes**
   - Modify the program to prevent the bugs identified by your test cases.
   - Verify that the fixed program does not crash or hang when run against the same test cases.

2. **Regression Testing**
   - Add your new test cases to the `testcases/valid` or `testcases/invalid` directories:
     - **Valid Test Cases**: Inputs the program should handle correctly.
     - **Invalid Test Cases**: Inputs that should trigger error handling (but not crashes).
   - Update the test suite to include these cases.

3. **Automate Testing**
   - Use **GitHub Actions** to ensure that future changes do not reintroduce bugs:
     - Set up a workflow to automatically run `make test` after every code change.
     - Ensure the workflow uses the updated test suite to verify the fixes.

---

### **Additional Notes**

- **Creating Test Files**
  - Use the provided Python scripts (`gengift.py`, `genanim.py`) to create binary gift card files for testing.

- **Exit Codes**
  - Programs in C/C++ often return an exit code (`return 0;` or similar) at the end of execution. A non-zero exit code typically signals an error. Perhaps a good practice to also implement in other languages?

- **Common Pitfalls**
  - Not all flagged issues by AddressSanitizer or UndefinedBehaviorSanitizer will lead to crashes. However, they indicate potential vulnerabilities that warrant investigation.

---

### **Deliverables**
1. Three test files (`crash1.gft`, `crash2.gft`, `hang.gft`) demonstrating unique flaws.
2. A detailed bug report in `part2.txt`.
3. Fixed source code with no crashes or hangs for the test cases.
4. Updated test suite and GitHub Actions workflow for regression testing. 

### Submission
Submit this part on gradescope, make sure to push the `hw1p2handin` tag to your repository first with the following:

    git tag -a -m "Completed hw1 part2." hw1p2handin
    git push origin main
    git push origin hw1p2handin
