# Part 1

## This is the buggy code
```
//chosen from ArrayExamples.java from week4's lab
static void reverseInPlace(int[] arr) {
  for(int i = 0; i < arr.length; i += 1) {
    arr[i] = arr[arr.length - i - 1];
  }
}
```

## 1. A failure-inducing input
```
//a method written in ArrayTests
@Test 
public void testReverseInPlace() {
  int[] input1 = {1, 2, 3, 4};
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{4, 3, 2, 1}, input1);
}
```

## 2. An input that doesn't induce a failure
```
//a method written in ArrayTests
@Test 
public void testReverseInPlace() {
  int[] input1 = { 3 };
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{ 3 }, input1);
}
```

## 3. The symptom, as the output of running the two tests above 
**Problem #1 Error message:**
```
%TESTC  1 v2
%TSTTREE1,testReverseInPlace(ArrayTests),false,1,false,-1,testReverseInPlace(ArrayTests),,
%TESTS  1,testReverseInPlace(ArrayTests)

%FAILED 1,testReverseInPlace(ArrayTests)
%TRACES 
arrays first differed at element [2]; expected:<2> but was:<3>
```

**Problem #2 Output:**
```
%TESTC  1 v2
%TSTTREE1,testReverseInPlace(ArrayTests),false,1,false,-1,testReverseInPlace(ArrayTests),,
%TESTS  1,testReverseInPlace(ArrayTests)

%TESTE  1,testReverseInPlace(ArrayTests)

%RUNTIME17
```

## 4. The bug, as teh before-and-after code change required to fix it

**The bug is that the arr copy the elements starting from its own ends. Once it reach to the midpoint, it will copy elements which have updated. This will result in a symmetric array.**

**buggy code before**
```
static void reverseInPlace(int[] arr) {
  for(int i = 0; i < arr.length; i += 1) {
    arr[i] = arr[arr.length - i - 1];
  }
}
```

**fixed code after**
```
static void reverseInPlace(int[] arr) {
  int[] reversedArr = new int[arr.length];
  for(int i = 0; i < arr.length; i += 1) {
    reversedArr[i] = arr[arr.length - i - 1];
  }
  for(int j = 0; j < arr.length; j += 1) {
    arr[j] = reversedArr[j];
  }
}
```

## 5. Briefly describe why the fix addresses the issue

**My fixed code has a new array that copy the elements in the original array in reversed order. This does not change the elements in the original array so there is no such problem that newly updated value is copied. Then the original array copy elements from the temporary array.**



# Part 2

## My choice is `grep` command

**1. `grep <<string>> <<file>>`** search a file or files for the given string, print matching lines.

**example 1**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep question biomed/1468-6708-3-3.txt
        (MIRACL) trial set out to answer this question.
        need for coronary revascularization, etc). The question of
        coronary syndrome, this remains an open question. Even if
```

**example 2**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep placebo biomed/1468-6708-3-4.txt
          parallel groups trial to compare placebo, candesartan
          placebo-controlled trial to assess treatment effect of
          were randomized after the placebo run-in phase.
          hydrochlorothiazide (HCTZ) plus placebo for the treatment
          of hypertension [ 5 ] . After 2 weeks of placebo lead in
          treatment (lovastatin or placebo), were contacted
```

**2. `grep -c <<string>> <<file>>`** count the number of occurance for the string in the given file.

**example 3**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep -c question biomed/1468-6708-3-3.txt
3
```

**example 4**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep -c placebo biomed/1468-6708-3-4.txt
6
```

**3. `grep -r <<string>> <<folder>>`** search the string recursively in files from the given folder.

**example 5**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep -r admission biomed/
biomed/1468-6708-3-3.txt:        63 hours) of admission for unstable angina or a non-Q-wave
biomed/1468-6708-3-3.txt:        than 270 mg/dL; Q-wave myocardial infarction on admission
biomed/1468-6708-3-3.txt:        revascularization in the months before admission, during
biomed/1468-6708-3-3.txt:        relatively low admission LDL cholesterol levels.
biomed/1471-2334-1-13.txt:        consecutive hospital admissions of 599 adults with HIV
biomed/1471-2334-1-13.txt:        within 24 hours of hospital admission, presence of blood
biomed/1471-2334-1-13.txt:        stream infection, intensive care unit admission status,
biomed/1471-2334-1-13.txt:        mortality rate for each hospital admission were calculated
......
```

**example 6**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep -r police 911report/
911report/chapter-10.txt:                of a military police lead vehicle and a van; the proposed briefing theater had no
911report/chapter-11.txt:                officials-local airport managers and local police departments- who had not seen such
911report/chapter-11.txt:                concerned citizens. Representatives of the Justice Department, the FAA, local police
911report/chapter-12.txt:                violent extremism. According to Karachi's police commander, there are 859 madrassahs
911report/chapter-12.txt:            The country's vast unpoliced regions make Pakistan attractive to extremists seeking
911report/chapter-12.txt:                with al Qaeda. Saudi police are regularly being killed in shootouts with terrorists.
911report/chapter-13.3.txt:                line workers, but not police or the Department of Corrections, from transmitting
911report/chapter-13.3.txt:            47. On the relationship between the FBI and state and local police forces, see
911report/chapter-13.3.txt:                screener. The terminal was evacuated, and police found miscellaneous gun parts,
......
```

**4. `grep -n <<string>> <<file>>`** display line numbers that matches the string in the given file

**example 7**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep -n police 911report/chapter-7.txt
375:                law-abiding citizen with long-standing, friendly contacts among local police and FBI
561:                police and a UAE representative tried to find him in Germany, visiting mosques and
564:                in Hamburg. The UAE government then told the Hamburg police they could call off the
682:                Arabia that borders Yemen; this weakly policed area is sometimes called "the wild
```

**example 8**
```
Bosen@Bosen-Gaming-Laptop MINGW64 ~/docsearch/technical (main)
$ grep -n Marriott 911report/chapter-9.txt
918:            FDNY South Tower and Marriott Hotel Operations.
969:                indirect ways to enter that tower, most often through the Marriott Hotel, or simply
975:                confusion over how to get to there, he instead ended up in the Marriott at about
978:                search-and-rescue operations on the upper floors of the Marriott. Four of these
982:            Feeling satisfied with the scope of the operation in the Marriott, the chief in the
985:                the NorthTower from the Marriott found a working elevator in a bank at the south end
992:                the tunnel already and had responded to the Marriott, not the South Tower.
1019:                Marriott. Third, efforts to track units by listening to tactical 1 were severely
1207:                and civilians-in the concourse, in the Marriott, and on neighboring streets. The
1209:                debris cloud. The Marriott hotel suffered significant damage as a result of the
1224:                and posts in the NorthTower lobby, the Marriott lobby, and the staging area on West
1323:            The Marriott Hotel suffered significant damage in the collapse of the South Tower.
1346:                radioed for two ladder companies to respond to the Marriott, where he was aware that
1361:                mission in the Marriott, "I would never think of myself as a leader of men if I had
1368:                civilians and firefighters from the Marriott.
1731:                complex. As a result, numerous units were congregating in the undamaged Marriott
```

## Citation
@ChatGPT

**my prompt**

what does `grep -n <<string>> <<file>>` do in bash?

**ChatGPT output**

The command grep -n "string" file in Bash is used to search for the specified "string" within the contents of the given file and display the matching lines along with their line numbers.

Here's a breakdown of each part of this command:

grep: This command is used in Unix/Linux environments to search for patterns within files.

-n: This option tells grep to display the matching lines along with their line numbers in the file. Each line of output will be prefixed with its line number.

"string": This is the pattern you are searching for within the file.

file: This specifies the file(s) in which grep will search for the specified "string". You can specify a single file or use wildcards (like *.txt) to search multiple files.
