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

