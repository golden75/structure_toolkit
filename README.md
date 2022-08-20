# structure_toolkit  


file name correction using:  

So I would recommend to use a small for loop and set the ISF system variable (if not the blanks will interfere). That leads you to something like this:
```
IFS=$(echo -en "\n\b")
```

remove "," "( )" 
```
remove ,
for file in $(find . -name "*.sdf"); do mv $file $(echo $file | sed -e s/,/_/); done;

remove ()
for file in $(find . -name "*.sdf"); do mv $file $(echo $file | sed -e s/\(//); done;

for file in $(find . -name "*.sdf"); do mv $file $(echo $file | sed -e s/\)//); done;

remove spaces
for f in *\ *; do  echo $f; mv "$f" "${f// /_}"; done

remove ,
for f in *,*; do echo $f; mv "$f" "${f//,/_}"; done

remove (
for f in *\(*; do echo $f; mv "$f" "${f//\(/}"; done

for f in *\)*; do echo $f; mv "$f" "${f//\)/}"; done


```


## 01 Convert the sdf to gaussian input files 
To run `sdf_2_gaussian.sh` it needs: `nmr.txt` and `opt.txt` files 

This will create
```
├── 01a_xyz
├── 02_gaussian_input
├── 02a_chk
├── 02b_gauslog
└── scripts/
        | -- samples.txt
```

sample.txt file will contain the gussian input file names, which will be used in running the gaussian array job

## 02 Optimize and calculate nmr

`02_gauss_array.sh` is a arrary script which takes the sample.txt file as its input.  It will use the gaussin input files located at 03_gaussian_input folder.

```
sbatch gauss_array.sh sample.txt
```  

This will produce the optimized structures and the calculated nmr.  
```
├── 02a_chk
├── 02b_gauslog
```

## 03 Extract optimized structures and convert to sdf  

gaussianLog_2_sdf.sh will take the gaussian log files and extract the xyz coordinates from it. Then it will convert it to sdf format.  

```
gaussianLog_2_sdf.sh 
```  

This will create the optimized structures in 
```
../
├── 03a_optimized_sdf
```  

## 04 Extract shift  

`extract_shift.sh` will take the gaussian log files and extract the nmr shifts into a txt file

```
../
├── 04_nmr
```  

## 05 Extract zmatrix   

`gaussianLog_2_gzmatrix.sh` will use the gaussian log files and extract the zmatrix.  

```
../
├── 05_gaus_zmatrix
```  

## 06 Extract optimized geomatry and convert it to xyz and gmatrix and sdf

`gaussianLog_2_noH-xyz.sh` will use gaussian log files and create xyz file with out H atoms. Using the xyz it will create the gmatrix file.  

```
├── 06a_opt_xyz
├── 06b_opt_noH-xyz (this contains both the xyz and gmatrix files)
```

`noH-xyz_2_noH-sdf.sh` will now take the xyz files with out H and convert it to sdf files.  

```
├── 06c_opt_noHSDF
```  

