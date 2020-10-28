### The utilization of cmake and the rmath library
- (on Ubuntu): sudo apt install cmake
- https://github.com/statslabs/rmath

### Build project 
Do this in same folder as CMakeLists.txt & source.c

```
# Build project (it will remove old build folders)
./build.sh

# Test program
echo -e "0.9\n0.5\n0.1e-10" | ./build/stat_r_in_c qnorm

# Test skip first row using -h1 (-h2 or -h3 to skip 2 or 3 lines)
echo -e "PVAL\n0.9\n0.5\n0.1e-10" | ./build/stat_r_in_c qnorm -s1

# Select which columns are index arg1 and arg2 (need tab separation)
echo -e "0\tpval\tOR\n1\t0.9\t1.2\n2\t0.5\t1.3\n3\t0.1e-10\t1.1"
echo -e "0\tpval\tOR\n1\t0.9\t1.2\n2\t0.5\t1.3\n3\t0.1e-10\t1.1" | ./build/stat_r_in_c qnorm -s1 -i 1 -p 2 -o 3

# calculate zscore from pvalue and odds-ratio
echo -e "0\tpval\tOR\tAF\n1\t0.9\t1.2\t0.4\n2\t0.5\t1.3\t0.6\n3\t0.1e-10\t1.1\t0.1" | ./build/stat_r_in_c pval_OR_2_Zscore --skiplines 1 --index 1 --pvalue 2 --oddsratio 3

```

### Build singularity image
First make sure singularity is installed

```
# Make singularity image based on defintion file
fname="$(date +%F)"-ubuntu-1804_stat_r_in_c.simg
sudo singularity build ${fname} ubuntu-18.04_stat_r_in_c.def 


# Check that image is executable and then test it (change date)
echo -e "0.9\n0.5\n0.1e-10" | ./20xx-xx-xx-ubuntu-1804_stat_r_in_c.simg stat_r_in_c qnorm
```

### Generate 1 million rows test data
```
./gentestdata.sh 1000000 > testdata

