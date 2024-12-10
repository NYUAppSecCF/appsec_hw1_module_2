### **Assignment 1, Module 2: Auditing and Test Cases**

This assignment focuses on understanding and improving the security of a C program (`giftcardreader.c`) and its accompanying header file (`giftcard.h`). You will audit the program, identify flaws, and create test cases to expose them. Additionally, you'll remediate the identified issues and implement regression tests to ensure the fixes persist.

---

- [Step 0: Pulling in Your Code](#step-0-pulling-in-your-code)
- [Step 1: Understand the Program](#step-1-understand-the-program)
- [Step 2: Find and Document Flaws](#step-2-find-and-document-flaws)
- [Step 3: Fix the Bugs](#step-3-fix-the-bugs)
- [Additional Notes](#additional-notes)
- [Deliverables](#deliverables)
- [Submission](#what-to-submit)


---

### **Step 0: Pulling in Your Code**

In this course, each module builds upon the work you completed in the previous module. This approach reinforces the importance of iterative development and allows you to refine your skills incrementally. The commands below are essential for integrating your progress from Module 1 into your current work. They ensure that you have access to the latest updates from your own repository so you can seamlessly continue with Module 2.

---

### **Why Is This Done?**
1. **Build on Previous Work**: Each module assumes that you’ve completed the foundational tasks from the previous one. For instance, in Module 1, you set up your repository and implemented initial features. In Module 2, you will extend this by auditing the program, creating test cases, and implementing fixes.
   
2. **Version Control**: Git allows you to track changes and manage updates to your code efficiently. By pulling your previous work, you maintain a synchronized development environment.

3. **Collaboration Readiness**: Even if you're working solo, you follow a similar workflow to collaborative projects, preparing you for real-world software development practices.

---

### **Commands Explanation and Verification**

The following commands will pull your Module 1 work into your current repository for Module 2. Let’s break them down:

```bash
git remote add upstream https://github.com/NYUAppSecCF/appsec_hw1-YOURPART1REPO
```
- **Purpose**: Adds the remote repository URL for Module 1 as an additional "upstream" source.
- **Why?**: You need a way to fetch changes from your previous work, and this command links the repository for Module 1 to your current project.
- **Check**: Ensure the URL `https://github.com/NYUAppSecCF/appsec_hw1-YOURPART1REPO` matches the repository for your Module 1 code. Replace `YOURPART1REPO` with your actual repository name.

```bash
git fetch upstream
```
- **Purpose**: Downloads the changes from the `upstream` repository to your local Git environment.
- **Why?**: This brings in the latest updates from Module 1 but does not automatically apply them yet.

```bash
git merge upstream/main --allow-unrelated-histories
```
- **Purpose**: Merges the `main` branch from your Module 1 repository into your current repository.
- **Why?**: Since Module 1 and Module 2 repositories may have different histories, the `--allow-unrelated-histories` flag ensures that the merge proceeds without errors due to this difference.

```bash
git push
```
- **Purpose**: Uploads the merged changes to your remote repository for Module 2.
- **Why?**: This ensures that your Module 2 repository reflects the updates from Module 1 and serves as your new starting point.

---

### **Important Notes**
1. **Adjust Repository Names**: If your repository name  is obviously different! Replace `YOURPART1REPO` with the correct name in the commands.
2. **Resolve Merge Conflicts**: If conflicts occur during the merge, Git will prompt you to resolve them manually before proceeding.
3. **Test After Pulling**: After pulling the updates, verify that your code builds and runs as expected. This step helps confirm that the merge was successful and introduced no issues.

By following this process, you’ll be well-prepared to continue your work in Module 2 without losing the progress made in Module 1! Future modules will have you run through the same steps, so do not forget! 

As always, make sure you understand why we're doing this!

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

### ### Ready for Submission
Submit this part on gradescope, make sure to push the `hw1p2handin` tag to your repository first with the following:

    git tag -a -m "Completed hw1 part2." hw1p2handin
    git push origin main
    git push origin hw1p2handin
    
## What to Submit

To submit your code, please only submit a file called `git_link.txt` that contains the name of your repository to **Gradescope**.
For example, if your repo is located at 'h<span>ttps:</span>//github.com/NYUAppSecCF/appsec-homework-1-module-1-exampleaccount', you would submit a text file named `git_link.txt` with only line that reads with <ins><b>only</b></ins> the following:

    appsec-homework-1-exampleaccount

Remember that <b>Gradescope is not instant</b>. Especially if we have to look into past GitHub action runs. We have a timeout set for 10 minutes, almost all well running code will complete within 5 minutes. Wait for it to complete or timeout before trying to re-run. 

For ease of grading, we ask that you also submit copies of your writeups as part2.txt directly in Gradescope. Please ensure that these writeups are exact copies of the files from your repository, as we have implemented a check to verify the match. **It cannot be blank, this will cause a hash failure!**. For further details on the writeup requirements, please refer to the grading rubric available in Brightspace under the "Assignment Guideline" section.

Your repository should contain:    
* Module 1
  * Your `.github/workflows/hello.yml`
  * At least one signed commit
  * Your modified C code `module1.c'
  * Your commpiled `module1.nyu`
* Module 2
  * In `testcases/invalid`: `crash1.gft`, `crash2.gft`, and `hang.gft`.
  * A text file named `part2.txt` that contains the bug descriptions
    for each of the three test cases
  * A GitHub Actions YML that runs your tests
  * A commit with the fixed version of the code (if you like, this
    commit can also contain the files mentioned above)
