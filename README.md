# DevOps-M2
It's repo for DevOps Milestone 2.


## Advanced Testing: fuzz testing `sun.js`
Although we've tested in project in step 1, we found that in `sun.js`, some statements and branches are not covered.  
Consequently, we applied the fuzz testing to `sun.js`. The test cases were added into the original test suite which we used in step 1. Scripts for fuzz testing see [here](https://github.ncsu.edu/DevOps-Milestones/solar-calc/blob/master/test/fuzz.js)

To run fuzz testing,
```
git clone https://github.ncsu.edu/DevOps-Milestones/solar-calc
npm install
npm run fuzz
```

Coverage report before we applied fuzz testing can be found [here](https://github.ncsu.edu/DevOps-Milestones/DevOps-M2/blob/master/sun1.pdf)  
After the fuzz testing, we got a better coverage, see [here] (https://github.ncsu.edu/DevOps-Milestones/DevOps-M2/blob/master/sun2.pdf)

![]()
