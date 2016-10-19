# DevOps-M2
It's repo for DevOps Milestone 2.
We use [solar-calc](https://github.ncsu.edu/DevOps-Milestones/solar-calc) as our open-source project. It is a sunrise and sunset calculator for npm based on the NOAA Solar Calculator.

 - Open-source project: [link](https://github.ncsu.edu/DevOps-Milestones/solar-calc)
 - Screencast: [link](https://youtu.be/wGa4Z-wqgxs)

### Prerequisite
At very beginning, you need to clone the repository and install the necessary packages.
Here are commands.
```
git clone https://github.ncsu.edu/DevOps-Milestones/solar-calc
sudo npm install
```

### Test Section
#### Test Suites: `The ability to run unit tests, measure coverage, and report the results`
Solar-calc has its own test suite, which locates in `test/test.js` file. Solar-calc uses `mocha` javascript test framework. And we combined `mocha` and `istanbul`. So you can run the original test suite and see the coverage information by typing the command below.
```
npm run test
```

The results are show below:
```
  suncalc
    2015-03-08 in North Carolina
      ✓ get solar noon
      ✓ get golden hour start
      ✓ get golden hour end
      ✓ get night end
      ✓ get nautical dawn
      ✓ get dawn
      ✓ get sunrise
      ✓ get sunriseEnd
      ✓ get sunsetStart
      ✓ get sunset
      ✓ get dusk
      ✓ get nautical dusk
      ✓ get night start
      ✓ should get moon illuminosity
      ✓ should get moon distance
    2015-06-23 in extreme latitude
      ✓ get solar noon
      ✓ get golden hour start
      ✓ get golden hour end
      ✓ get night end
      ✓ get nautical dawn
      ✓ get dawn
      ✓ get sunrise
      ✓ get sunriseEnd
      ✓ get sunsetStart
      ✓ get sunset
      ✓ get dusk
      ✓ get nautical dusk
      ✓ get night start
      ✓ should get moon illuminosity
      ✓ should get moon distance

  30 passing (209ms)

=============================== Coverage summary ===============================
Statements   : 95.47% ( 358/375 )
Branches     : 70.24% ( 59/84 )
Functions    : 100% ( 74/74 )
Lines        : 97.21% ( 314/323 )
================================================================================
```
As you can see the statement coverage is already very high. However, if you try to open the reports automatically generated by istanbul by typing the command below, you will see some lines are still not covered. 
```
open coverage/lcov-report/solar-calc/lib/index.html
```

#### Advanced Testing: `Fuzz testing sun.js`
Although we've tested the project in step 1, we found that in `sun.js`, some statements and branches are not covered.  
Consequently, we applied the fuzz testing to `sun.js`. The test cases were added into the original test suite which we used in step 1. Scripts of fuzz testing is in [test/fuzz.js](https://github.ncsu.edu/DevOps-Milestones/solar-calc/blob/master/test/fuzz.js).

To run fuzz testing, you need to type the command below.
```
npm run fuzz
```

Coverage report before we applied fuzz testing can be found [here](https://github.ncsu.edu/DevOps-Milestones/DevOps-M2/blob/master/sun1.pdf).  
After the fuzz testing, we got a better coverage, see [here] (https://github.ncsu.edu/DevOps-Milestones/DevOps-M2/blob/master/sun2.pdf).

![](https://github.ncsu.edu/DevOps-Milestones/DevOps-M2/blob/master/sun_fuzz.png)  
(up: sun.js coverage info before and after the fuzz testing; down: sun.js coverage details before and after the fuzz testing)

The following figure shows the coverage status from Line 44 through line 88 before and after the fuzz testing.
![](https://github.ncsu.edu/DevOps-Milestones/DevOps-M2/blob/master/sun_detail.png)  
(left: test suite ouput before fuzz testing; right: test suite output after fuzz testing)

### Analysis Section
#### Basic Analysis: `The ability to run an existing static analysis tool on the source code and report its findings.`
We use [ESlint](https://github.com/eslint/eslint) as the static analysis tool. ESLint is a tool for identifying and reporting on patterns found in ECMAScript/JavaScript code. In many ways, it is similar to JSLint and JSHint.
You can run the ESlint easily and check the quality of source code.The command is below.
```
npm run lint
```
Then you will get the output from terminal.
```
src/moon.js
  302:11  warning  i is already defined      no-redeclare
  303:8   warning  Eterm is already defined  no-redeclare

✖ 2 problems (0 errors, 2 warnings)
```
The output shows 2 warnings existing in moon.js saying two variables are already defined. One more thing, ESLint is designed to be completely configurable. And we can change its configuration by modifying [.eslintrc](https://github.ncsu.edu/DevOps-Milestones/solar-calc/blob/master/.eslintrc) file.

#### Custom Metrics: `The ability to implement custom source metrics`
We created a file named [custom_metrics.js](https://github.ncsu.edu/DevOps-Milestones/solar-calc/blob/master/custom_metrics.js). And we implemented several custom metrics by using `esprima`. They are
 - Max condition: Count the max number of conditions within an if statement in a function;
 - Long method: Here we defined that if one method is more than 20 LOC, it is a long method;
 - SimpleCyclomaticComplexity: Number of if statements/loops + 1;
 - Duplicate code detection: Detect duplicated code from source code.
You can check the source code by running below commend. It will analysis the `src/solarCalc.js` file by default.
```
npm run metrics
```
If you want to check other files, you can add `filePath` option.
```
npm run metrics --filePath=src/sun.js
npm run metrics --filePath=src/moon.js
```
The output is like below. You can see the script will print the name, first line, max conditions of each method in certain source file. And the script also check whether certain method is a long method or not and calculate the cyclomatic complexity of each method.

```
=====File Path: src/sun.js====================
constructor(): 4
============
MaxConditions: 0	LongMethod: false		SimpleCyclomaticComplexity: 1

... ...

calcSunriseSet(): 244
============
MaxConditions: 6	LongMethod: true		SimpleCyclomaticComplexity: 3


calcJDofNextPrevRiseSet(): 267
============
MaxConditions: 0	LongMethod: false		SimpleCyclomaticComplexity: 1
```

The script will also check the duplicated code from source code. As you can see, the script found three duplicated code pairs from source code.

```
=====Duplicated Code Detection====================

  Match - 2 instances

- ./src/moon.js:28,37
+ ./src/sun.js:180,189
   function getJD(date) {
     var year = date.getFullYear();
     var month = date.getMonth() + 1;
     var day = date.getDate();
   
     var A = Math.floor(year / 100);
     var B = 2 - A + Math.floor(A / 4);
     var JD = Math.floor(365.25 * (year + 4716)) + Math.floor(30.6001 * (month + 1)) + day + B - 1524.5;
     return JD;
   }

  Match - 2 instances

- ./src/moon.js:190,195
+ ./src/moon.js:234,239
-  var T45AD = [0, 2, 2, 0, 0, 0, 2, 2, 2, 2,
-    0, 1, 0, 2, 0, 0, 4, 0, 4, 2,
-    2, 1, 1, 2, 2, 4, 2, 0, 2, 2,
-    1, 2, 0, 0, 2, 2, 2, 4, 0, 3,
-    2, 4, 0, 2, 2, 2, 4, 0, 4, 1,
-    2, 0, 1, 3, 4, 2, 0, 1, 2, 2];
+  var T45BD = [0, 0, 0, 2, 2, 2, 2, 0, 2, 0,
+    2, 2, 2, 2, 2, 2, 2, 0, 4, 0,
+    0, 0, 1, 0, 0, 0, 1, 0, 4, 4,
+    0, 4, 2, 2, 2, 2, 0, 2, 2, 2,
+    2, 4, 2, 2, 0, 2, 1, 1, 0, 2,
+    1, 2, 0, 4, 4, 1, 4, 1, 4, 2];

  Match - 2 instances

- ./src/sun.js:39,44
+ ./src/sun.js:215,220
   if (z < 2299161) {
     A = z;
   } else {
     var alpha = Math.floor((z - 1867216.25) / 36524.25);
     A = z + 1 + alpha - Math.floor(alpha / 4);
   }

 3 matches found across 3 files
 ```

#### Gates: `Using hooks to reject a commit if it fails a minimum testing criteria and analysis criteria`
We created a `pre-commit` hook to make sure source code following testing criteria and analysis criteria. The rules are listed below.
 - If Statement coverage is less than 95.5%, commit will be refused.  
 - If errors exist, commit will be refused.  
 - If warnings exist, commit will be refused.  
Below is the content of this pre-commit hook.
```shell
#!/bin/sh
echo "Start pre-commit"


s2=$(npm run fuzz)
s3=$(npm run lint)

s21=$(echo "$s2" | grep -Eo "Statements.*\d\d%" | grep -o '[^ ]*%')
s31=$(echo "$s3" | grep -Eo "\d problems" | grep -o '[^ ]*')
s32=$(echo "$s3" | grep -Eo "\d errors" | grep -o '[^ ]*')
s33=$(echo "$s3" | grep -Eo "\d warnings" | grep -o '[^ ]*')

# remove '#'
# echo $s12
s22="${s21%?}"
# echo $s22
# echo $s31
echo $s32
echo $s33

# echo "$s12 < 94.9" |bc -l
# echo "$s12 < 95.9" |bc -l

if [ $(echo "$s22 < 95.5" |bc -l) -eq 1 ]; then
  echo "$s22% Statements coverage is smaller than threshold 95.5, commit refused!"
  exit 1
else
  echo "$s22% Statements coverage is larger than threshold"
fi

if [ -z "$s32" ] && [ -z "$s33" ]; then
  echo "There is no error and no warning"
else
  if [ $(echo "$s32 > 0" |bc -l) -eq 1 ]; then
    echo "The code contains $s32 errors, commit refused!"
    exit 1
  else
    echo "The code contains no error"
  fi

  if [ $(echo "$s33 > 0" |bc -l) -eq 1 ]; then
    echo "The code contains $s33 warnings, commit refused!"
    exit 1
  else
    echo "The code contains no warning"
  fi
fi


echo "End pre-commit"
exit 0
```
And we also used Jenkins `Post build task` plugin to run another [analysing script](https://github.ncsu.edu/DevOps-Milestones/solar-calc/blob/master/post_build.py) and generate a simple report. Below is the content of this report.
```
========== Analysis Report ==========
Statement Coverage: 96, good job!
No error or warning
========== End of Report ==========

POST BUILD TASK : SUCCESS
END OF POST BUILD TASK : 0
Finished: SUCCESS
```
