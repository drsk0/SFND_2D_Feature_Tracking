# Midterm Project Writeup

## Data Buffer

### MP.1 Data Buffer Optimization

This point has been addressed by replacing `std::vector` with `std:deque`.
Elements are pushed to the end of the deque and elements at front are dropped
while the ring buffer is bigger than the specified size.

## Keypoints

### MP.2 Keypoint Detection

The algorithms HARRIS, FAST, BRISK, ORB, AKAZE, and SIFT can be specified on
the command line and a dispatch to the respective OpenCV implementation has
been added. The HARRIS implementation is taken from the exercises.

### MP.3 Keypoint Removal

Keypoints outside of the specified rectangle are filtered out after detection
via a call to `std::copy_if`.

## Descriptors

### MP.4 Keypoint Descriptors

The algorithms BRIEF, ORB, FREAK, AKAZE and SIFT can be specified on the command
line and a dispatch to the respective OpenCV implementation has been added.

### MP.5 Descriptor Matching

FLANN/knn matching are implemented as shown in the exercises. A dispatch has
been added, such that the matching algorithm can be specified on the command
line.

### MP.6 Descriptor Distance Ratio

The descriptor distance ratio test has been implemented as shown in the
exercises with the hyper-parameter 0.8.

## Performance

### MP.7 Performance Evaluation 1

The following table shows the number of extracted keypoints in the specified
area. These numbers are rough estimates of the mean across the 10 images.

Detection Algorithm | Nr     | Distance Mean | Distance Variance
------------------- | -------| ------------- | -----------------
SHITOMASI           |   120  | 4             | 0
HARRIS              |   30   | 6             | 0
FAST                |   367  | 7             | 0
BRISK               |   276  | 22            | 3
ORB                 |   116  | 56            | 5
AKAZE               |   167  | 7.7           | 1.2
SIFT                |   139  | 5.2           | 0.3 


### MP.8 Performance Evaluation 2

The following table shows averages of number of matches across the 10 images.
Note that the AKAZE descriptor extractor only works with keypoints found by the
AKAZE algorithm.

|        | SHITOMASI | HARRIS | FAST | BRISK | ORB | AKAZE | SIFT
|------- | --------- | ------ | ---- | ----- | --- | ----- | ----
| BRIEF  |   81      |   25   | 196  | 121   | 41  | 121   | 66
| ORB    |   76      |   19   | 184  | 66    | 46  | 102   | out of memory
| FREAK  |   58      |   19   | 158  | 98    | 32  | 107   | 56
| AKAZE  | -         | -      | -    | -     | -   | 130   | -
| SIFT   |   90      |   14   | 250  | 149   | 68  | 141   | 89

### MP.9 Performance Evaluation 3

This tables shows the average time for keypoint detection across the 10 images.

Detection Algorithm | Time in ms
------------------- | ----------
SHITOMASI           | 16
HARRIS              | 23
FAST                | 5.6
BRISK               | 46
ORB                 | 19
AKAZE               | 82
SIFT                | 104

This table shows the time for descriptor creation. The time was measured on the
keypoints found by the AKAZE algorithm.

Descriptor Algorithm | Time in ms
-------------------  | ----------
BRIEF                | 1.5
ORB                  | 4.8
FREAK                | 30
AKAZE                | 44
SIFT                 | 24

#### Conclusion

The top 3 keypoint/descriptor algorithm pair for the purpose of designing a collision detection system are:

Rank | Keypoint Algorithm | Descriptor Algorithm 
---- | ------------------ | -------------------
1    | FAST               | BRIEF
2    | FAST               | ORB
3    | FAST               | SIFT

This ranking puts heighest weight on the speed of the resulting algorithm
combination. Here FAST is the clear winner for keypoint extraction. BRIEF is the
fastest descriptor algorithm, but still gives a good number of matches. Second
fastest is ORB with a similar number of matches. SIFT is quiet a bit slower, but
in turn creates a significant higher number of matches.

To have a better quality of the match number, good versus false positive should
be taken into account. This could be accomplished by an F1 score.
