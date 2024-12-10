# Assignment 1 Module 2: Auditing and Test Cases

Read through the giftcardreader.c program (and its accompanying header, giftcard.h) to get a feel for it. You should also try building and running it with the included examplefile.gft file, to see what its normal output is. You may find it helpful to use a debugger like gdb or lldb to step through the program as it executes.
Take the time to become familiar with these types of debuggers, or use the tools of your choice. Either way, what you need to do at this point is go through the code, read it, and try and understand how it works. One thing that helps many is to write down how the different parts are connected, either via a visual diagram or text. 

There is also a Makefile included in the repository. You can use this to build the program by typing make.

You can also use the Makefile to run the (very minimal and incomplete) test suite (to which you will be adding more tests!) for the program by typing make test, which uses runtests.sh to run the gift card reader on the gift cards found in testcases/valid and testcases/invalid. 
Take the time to understand what a makefile is, what runtests.sh does, and what a testcases/valid and testcases/invalid folder may contain. Again, a 'valid' test case, should be a test that works, as expected. As in, it's **valid**. An invalid test case results in an unexpected behavior by the program. As in, it's *invalid**. 
While you may understand what causes the behavior, so it's 'expected behavior' for you. The classification is still 'unexpected behavior' from the viewpoint of the application itself. 

The Make file will compile the following three binaries for you each time you run it:

giftcardreader.original - This is the giftcard reader without any modifications
giftcardreader.asan - This executable has a compiler flag -fsanitize=address that tells the compiler to use the AddressSanitizer, a memory error detector.
giftcardreader.ubsan - This has a compiler flag -fsanitize=undefined that tells the compiler to use the UndefinedBehaviorSanitizer, a fast undefined behavior detector. It helps detect undefined behavior issues like integer overflows, misaligned or null pointers, etc.


For this part, your job will be to find some flaws in the program, and then create test cases (i.e., binary gift cards) that expose flaws in the program. Use these binaries to your advantage to identify potential flaws. This requires a considerable amount of work on your end tracing through the code (hence the usefulness of a debugger) to try and identify the issues. 

You should write (create):

Two test cases, crash1.gft and crash2.gft, that cause the program to crash; each crash should have a different root cause. Make sure you understand the causes for your write-up. If your input gift card can crash the **original giftcard reader program** you get credit. The Asan and UBSAN binaries exist to help you spot potential crashes or risks. Just keep in mind, just because a compiler flag tells you there is an error, doesn't mean exploiting it will result in a crash, sometimes it results simply in unexpected behavior (and potential access). To get credit for remediation, you must at a MINIMUM ensure that the input gift card file that you used to create a crash no longer does. Sometimes a remediation is implemented poorly and creates a new risk, **you need to be mindful of this!**. To check if a program crashed, you will see a Segmentation (or other) fault. This can be verified checking the exit code of a program using $? on the terminal after your giftcardreader has finished executing. 

Think about why so many C/C++ programs 'return 0;' at the end of a function, this is our exit code. Zero implies typically everything is 'okay' a non-zero return tends to be due to an error of somekind.

One test case, hang.gft, that causes the program to loop infinitely. (Hint: you may want to examine the "animation" record type to find a bug that causes the program to loop infinitely.)

A text file, part2.txt explaining the bug triggered by each of your three test cases.

To create your own test files, you may want to refer to the gengift.py and genanim.py programs, which are Python scripts that create gift card files of different types.

Finally, fix the bugs that are triggered by your test cases, and verify that the program no longer crashes / hangs on your test cases. To make sure that these bugs don't come up again as the code evolves, have GitHub Actions automatically build and run the program in your test suite. You can do this by placing the new test cases in the testcases/valid or testcases/invalid directories (depending on whether the gift card reader should accept or reject them). Then have GitHub Actions run make test. Note that you do not need to run your tests on the unfixed version of the code---the tests are intended to verify that the code is fixed and prevent the bugs from being reintroduced in later versions (known as regression tests). You are showing that your fixes work, which means running your newly built code.
