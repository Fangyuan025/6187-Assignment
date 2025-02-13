# Problem 1

Our aim in this assignment is unit testing, coverage analysis of unit tests, and evaluation of existing tools for the automated unit testing. We will use an open source project [*Java : Algorithms and Data Structure*](https://github.com/phishman3579/java-algorithms-implementation) as our case study. The project is written in Java, uses [Apache Ant](https://ant.apache.org/) for the build, and uses JUnit to develop and execute tests. We will evaluate a coverage tool (Jacoco), and unit test generators [EvoSuite](https://www.evosuite.org), and [Randoop](https://randoop.github.io/randoop/). You may use an IDE of your choice for this assignment, however, we only provide command line instructions when it is necessary. Please understand that we CANNOT ADDRESS any questions concerning IDE, GIT, or installation of the required tools for this assignment. Successful installation and use of the tools are considered for the grading of the assignment. Also, note that the commands have only been tested on Mac, so they might differ slightly for Windows.



############## Part 1 [5 points] Setup an Environment to Write and Execute Tests Code using *JUnit*. 

**Prerequisites:** We assume you have already installed Git and Java JDK ("1.8") in your system.
**Assumption:** Let us assume `ws` is a directory that will contain all of the files and source code.
**Note:** For the answer to this question, you should only include output of Ant for Building code.

## Download Code

1. `cd ws`  
2. `git clone https://github.com/phishman3579/java-algorithms-implementation.git`  
3.  Get familiar with the source code of the project. Briefly explain what the source code contains.   


## Build Code
1. Download and install JDK 8 (1.8) (https://adoptium.net/temurin/releases/?version=8&os=any) and make sure JAVA_HOME is set accordingly. Note that some of the tool in this assignment only work with JDK 8 (1.8). Ensure you java version is correct using 

```bash
java -version 
openjdk version "1.8.0_442"
OpenJDK Runtime Environment (Temurin)(build 1.8.0_442-b06)
OpenJDK 64-Bit Server VM (Temurin)(build 25.442-b06, mixed mode)

```

2. Download Ant (https://ant.apache.org/bindownload.cgi), and save it in /ws/ant.  
3. Set Java_Home environment variable to where Java JDK bin and libs are located.   
4. `cd  ws/java-algorithms-implementation`.
5. Run `ws/ant/bin/ant tests_jar` (this will build the project).  
6. Check the location of the generated Jar file (`dist`) that is reported by Ant, and based on that, specify: How many Jar files have been generated? Explain what each Jar file includes. 
7. Check the build file (`build.xml`), and run the tests. How can you run the tests that are related to sort algorithms?


## Compile and Run the Tests JUnit

1. Download [JUnit](https://github.com/junit-team/junit4/wiki/Download-and-Install), and save it in `ws/junit`. Both `junit.jar` and `hamcrest-core.jar` files are required.  
2. Download [junit-platform-console-standalone](https://mvnrepository.com/artifact/org.junit.platform/junit-platform-console-standalone/1.9.0-RC1), and save it in `ws/junit`.  

3. Compile a test (e.g., `test@/com/jwetherell/algorithms/data_structures/test/AVLTreeTests.java`  
``` bash    
        cd ws/java-algorithms-implementation
        javac  -cp ../junit/junit.jar:src/:test/     test/com/jwetherell/algorithms/data_structures/test/AVLTreeTests.java   
```  
4. Execute the compiled test using *JUnit*  
```bash      
        cd ws/java-algorithms-implementation
        java  -cp ../junit/junit.jar:src/:test:../junit/hamcrest-core.jar  org.junit.runner.JUnitCore com.jwetherell.algorithms.data_structures.test.AVLTreeTests  
```  
  
5. The above command does not provide a detailed output of the tests. Let us use *junit-platform-console-standalone* to get a detailed report on the executed tests. 

```bash  
        cd ws/java-algorithms-implementation
        java  -jar  ../junit/junit-platform-console-standalone.jar  -cp src/:test --scan-classpath  --include-classname '.*'  --details verbose 

``` 

6. Explore the other options of *junit-platform-console-standalone*, and find out how we can run all the tests in a certain package. Why do we pass `--include-classname '.*'` in the above command? What are the other output formats provided by the *junit-platform-console-standalone*?  

7. Compile all tests in ws/test/com/jwetherell/algorithms/sorts/test/, and then run them using *junit-platform-console-standalone*. How many tests are included? Report the number of passed and failed tests. 


##############  Part 2 [10 points] Measuring the Coverage of Tests

Use the results of step 7 of the previous question, and answer the following questions.

1. Do research on a code coverage tool called *Jacoco*. Briefly explain how  *Jacoco* measures the code coverage.
2. Download the latest Jacoco from https://github.com/jacoco/jacoco/releases, unzip it, and  save it in ws/jacoco. Then, measure the coverage of the tests from Step 7 of the previous question.
    2.1 Generate the traces or AVLTree, using one of the following ways.
``` bash
    cd ws/java-algorithms-implementation
    java  -javaagent:../jacoco/lib/jacocoagent.jar  -cp test/:../junit/junit.jar:.:src/:../junit/junit.jar:../jacoco/lib/org.jacoco.core.jar:../junit/hamcrest-core.jar    org.junit.runner.JUnitCore    com.jwetherell.algorithms.data_structures.test.AVLTreeTests

    
 ```   
    
or using `junit-platform-console-standalone`

```bash
    cd ws/java-algorithms-implementation
    java -javaagent:../jacoco/lib/jacocoagent.jar=destfile=jacoco.exec -jar ../junit/junit-platform-console-standalone.jar -cp src/:test --scan-classpath --details verbose --include-classname '.*'


```

    2.2 Investigate what *javaagent* does in the previous step (2.1). 
    2.3 Generate the coverage report, using the following command

```bash
        cd ws/java-algorithms-implementation
        java -jar ../jacoco/lib/jacococli.jar  report jacoco.exec  --sourcefiles src/ --classfiles src/com/jwetherell/algorithms/data_structures/AVLTree.class  --csv testcov.csv
```   

    2.4 Review the testcov.csv, and briefly explain what it contains. 
    
    2.5 Measure the coverage of all tests in test/com/jwetherell/algorithms/data_structures/test/, and report their coverage. Note that we have already provided an example above. Try to write a script or find a command option of *Jacoco* to create the coverage report for all tests. 
    
    2.6 List classes with branch coverage of greater than 0 and less than 50%.  
    
    2.7 Briefly explain the four coverage metrics that *Jacoco* calculates. What is the difference between line and instruction coverage? 
    
    2.8 Which of the following statements are true? Justify your answer with logical reasoning, or a counterexample based on the results of step 2.5. Assume that inst(A), branch(A), line(A), and comp(A) return the  instruction, branch, line, and complexity coverage of class A. 
        2.8.1 Always inst(A) >= branch(A)
        2.8.2 If line(A)== 100 then inst(A) = 100
        2.8.3 If line(A)== 100 then branch(A) = 100
        2.8.4 A class that includes no method with a branching statement, has branch and instruction coverage of either 0 or 100
        
 
############## Part 3 [20 points] Develop Test Cases to Increase the Coverage of Existing Tests
	
1. Complete the existing tests  of Stack.ArrayStack, Matrix, SuffixTree.Edge, SuffixTrie to increase the test coverage of each class as much as possible. (100% is the preferred branch coverage).  
 
	
    **Hint** *Jacoco* provides an HTML report that can be very helpful in this context. We recommend reviewing the HTML report of Jacoco before starting to develop the tests. 
	
    **Note** For this assignment, submit your test code files (compressed) plus a coverage report for the classes, and brief explanation of your approach to increasing the coverage of tests. Note that your tests should complement the existing tests, and you are not allowed to remove any of the existing tests. 


##############  Part 4 [15 points] Automated Unit Testing

1. Do research on a tool called [EvoSuite](https://www.evosuite.org/). Briefly explain how EvoSuite generates unit tests. Note that, it seems that EvoSuite does not work with the latest version of Java. **EvoSuite only run with Java 1.8, if you skipped installing Java 8 at the first step, you need to do it now.** 


2. Download EvoSuite from https://github.com/EvoSuite/evosuite/releases/tag/v1.2.0, then unzip, and store it in `ws/evosuite`
3. To generate unit tests for a class (e.g., `com.jwetherell.algorithms.data_structures.SuffixTree`) follow these steps:

- Make sure the `JAVA_HOME` is set correctly.

- Generate tests:   
        `cd ws/java-algorithms-implementation`   
        `java    -jar ../evosuite/evosuite.jar  -target src/ -class   com.jwetherell.algorithms.data_structures.Graph`

- Compile the generated tests:  
        `javac  -cp .:src/:../junit/junit.jar:evosuite-tests/:../evosuite/evosuite.jar  evosuite-tests/com/jwetherell/algorithms/data_structures/*.java` 

- Run the generated tests:  
        `java  -jar  ../junit/junit-platform-console-standalone.jar  -cp ../evosuite/evosuite.jar:src/:evosuite-tests/ --scan-classpath  --details verbose`

---

4.  Do research on a tool called [Randoop](https://randoop.github.io/randoop/). Briefly explain how Randoop generates unit tests. 
5.  Download Randoop from  https://github.com/randoop/randoop/releases/download/v4.3.1/randoop-4.3.1.zip, then unzip and store it in ws/randoop
6.  To generate unit tests for a class (e.g., `com.jwetherell.algorithms.data_structures.AVLTree`) follow these steps:

- Make sure the `JAVA_HOME` is set correctly. 

- Generate tests:  
          `mkidr  ws/java-algorithms-implementation/randoop_tests & cd ws/java-algorithms-implementation/randoop_tests`   
          `java -cp    ../src:../../randoop/randoop.jar:../../junit/junit.jar:../../junit/hamcrest-core.jar    randoop.main.Main gentests --testclass  com.jwetherell.algorithms.data_structures.Graph  --time-limit=60`
  
- Compile the generated tests:  
        `javac  -cp .:../src/:../../junit/junit.jar ./*.java` 

- Run the generated tests:   
        `java  -jar  ../../junit/junit-platform-console-standalone.jar  -cp .:../../evosuite/evosuite.jar:../src --scan-classpath  --details verbose`

---
7. Based on the above instructions, generate unit tests for `SkipListMap`, `SuffixTrie`, `DisjointSet`, `TrieMap`, `Matrix`, and `Graph`. The above instruction is only a simple instruction to run the tool, but you are expected to learn the tool and tune it to get a better result.  

8. Compare the generated tests by the above two tools, based on ease of use, readability of the generated tests,  detected faults, any other related metric of your choice. Justify your answer by providing examples and evidence.
