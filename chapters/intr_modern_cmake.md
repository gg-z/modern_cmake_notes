# An introduction to Moden CMake  

## **Why do we need a good build system?**   
- avoid hard-coding paths
- build a package on more than one computer  
- use computer integration 
- support different OSs
- support multiple compilers  
- use an IDE  
- use a library  
- ....

## CMake Antipatterns   

### 1. Do not use global functions  
this include ```link_directories```, ```include_libraries```, and similar  

### 2. Do not add unneeded PUBLIC requirements    
You should avoid forcing somthing on users that is not required(```-Wall```), Make these PRIVATE instead.   

### 3. Do not GLOB files.  
Make or another tool will not know if you add files without rerunning CMake. 

### 4. Link to built file directly   
Always link to tragets if available.      

    
          

## CMake Patterns     

### 1. Treat CMake as code  
It is code, it should be as clean and readable as all other code.  

### 2. Think in targets  
Your target should represent concepts. Make an (IMPORTED) INTERFACE target for anything that should stay together and link to that.   

### 3. Export you interface  
You should be able to run from build or install   

### 4. Wirte a Config.cmake file   
This iis what a library anthor should do to support clients.  

### 5. Make ALIAS targets to keep usage consistent.  
Using ```add_subdirectory``` and ```find_package``` should provide the same targets and namespaces.  

### 6. Combine common functionality into clearly documented functions or macros  
Functions are better usually.  

### 7. Use lowercase function names  
CMake functions and macros can be called lower or uppter case. Always use lower case. Upper case if for variables/   

### 8. ```cmake_policy``` and range of versios  
Policies change for a reason. Only piecemeal set OLD polices if you have to.   

