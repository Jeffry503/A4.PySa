# A4.PySa
A4: PySa
Using: [Tutorial](https://github.com/facebook/pyre-check/tree/main/documentation/pysa_tutorial)

PySa is an open source static analysis tool to detect and prevent security issues in Python code.

Set Up
Since we would be using Ubuntu for this assigment, I decided to use an Ubuntu 22.04 virtual machine I had previously configured. Then, I followed the commands provided in the documentation.

git clone https://github.com/facebook/pyre-check.git
cd pyre-check

cd documentation/pysa_tutorial

python3 -m venv tutorial
source tutorial/bin/activate
pip3 install pyre-check fb-sapp django-stubs

Exercise 1:




Exercise 2:
sources_sinks.pysa
django.http.request.HttpRequest.GET: TaintSource[CustomUserControlled] = ...
django.http.request.HttpRequest.POST: TaintSource[CustomUserControlled] = ...

def eval(__source: TaintSink[CodeExecution], __globals, __locals): ...
def exec(__source: TaintSink[CodeExecution], __globals, __locals): ...

def subprocess.getoutput(cmd: TaintSink[ShellExecution]): ...

taint.config
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

