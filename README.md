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
To run `01_sdf_2_gaussian.sh` it needs: `nmr.txt` and `opt.txt` files 

This will create
```
├── 02_xyz
├── 03_gaussian_input
├── log_files
├── results
└── scripts/
        | -- samples.txt
```

sample.txt file will contain the gussian input file names, which will be used in running the gaussian array job

## 02 Optimize and calculate nmr

`02_gauss_array.sh` is a arrary script which takes the sample.txt file as its input.  It will use the gaussin input files located at 03_gaussian_input folder.

```
sbatch 02_gauss_array.sh sample.txt
```  

