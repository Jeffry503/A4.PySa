# A4: PySa

**Using:** [Tutorial](https://github.com/facebook/pyre-check/tree/main/documentation/pysa_tutorial)

PySA, or Python Static Analyzer, is a tool used for analyzing Python code statically to find potential issues, bugs, or code smells without executing the code. It performs various checks such as detecting syntax errors, looking for unused variables, identifying unreachable code, and more. PySA helps developers ensure code quality and maintainability by providing insights into potential problems early in the development process.
<br>

## Set Up

Since we would be using Ubuntu for this assigment, I decided to use an Ubuntu 22.04 virtual machine I had previously configured. Then, I followed the commands provided in the documentation. I don't know why I was not able to run pysa. I used a mac as a host machine with Ubuntu 22.04. However I was not able to figure it out. but it was a great experience to try the commands and how to access files trough Ubuntu terminal.

```
git clone https://github.com/facebook/pyre-check.git
cd pyre-check

cd documentation/pysa_tutorial

python3 -m venv tutorial
source tutorial/bin/activate
pip3 install pyre-check fb-sapp django-stubs
```
<br> 

## Exercise 1:

<img src='/Screenshot1.png' width=''/>

<img src='/Screenshot2.png' width=''/>

<img src='/Screenshot3.png' width=''/>


## Exercise 2:

### sources_sinks.pysa 
```
django.http.request.HttpRequest.GET: TaintSource[CustomUserControlled] = ...
django.http.request.HttpRequest.POST: TaintSource[CustomUserControlled] = ...

def eval(__source: TaintSink[CodeExecution], __globals, __locals): ...
def exec(__source: TaintSink[CodeExecution], __globals, __locals): ...

def subprocess.getoutput(cmd: TaintSink[ShellExecution]): ...
```
<br>

### taint.config
```
{
  "sources": [
    {
      "name": "CustomUserControlled",
      "comment": "use to annotate user input"
    }
  ],

  "sinks": [
    {
      "name": "CodeExecution",
      "comment": "use to annotate execution of python code"
    },
    {
      "name": "ShellExecution",
      "comment": "use to annotate execution of shell scripts"
    }
  ],

  "features": [],

  "rules": [
    {
      "name": "Possible RCE:",
      "code": 5001,
      "sources": [ "CustomUserControlled" ],
      "sinks": [ "CodeExecution" ],
      "message_format": "User specified data may reach a code execution sink"
    },
    {
      "name": "Possible Shell Execution:",
      "code": 9001,
      "sources": [ "CustomUserControlled" ],
      "sinks": [ "ShellExecution" ],
      "message_format": "User specified data may reach a shell execution sink"
    }
  ]
}
```
<br>


<img src='/Screenshot4.png' width=''/>
