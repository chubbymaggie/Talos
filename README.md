# Talos

Introduction

    CodeAnalyzer is a tool that uses data flow analysis and control flow analysis to collect various 
    information about a C/C++ program such as call graph, control dependency, function return value, 
    and how the return value of a function call is checked by the program. To generate a complete 
    call graph, CodeAnalyzer also keeps track of function calls via a function pointer. CodeAnalyzer 
    is implemented as an analysis pass of LLVM. For a C/C++ source file, CodeAnalyzer outputs the 
    collected information in text format, which is described in details in the MANUAL file. Choosing 
    text format makes it easier to process the output of CodeAnalyzer with Python or other programming 
    languages designed for rapid development.
    
    CodeAnalyzer is easy to install and use. It includes several helper scripts such as C/C++ compiler
    wrappers to faciliate analyzing large projects without the need to modify the configuration of
    the projects. The TUTORIAL file demonstrates how to build CodeAnalyzer and use it to analyze 
    the source code of lighttpd, a widely-used HTTP server. 
    
    So far CodeAnalyzer has been used to analyze 12 popular open-source applications such as HTTP 
    servers, FTP server, database server, programming language interpreter, web browsers, and network 
    protocol analyzer, with 100,000 lines of source code on average. The following is the list of 
    the applications that were successfully analyzed by CodeAnalyzer.
    
    - HTTP server/proxy: apache httpd, lighttpd, squid 
    - FTP server: proftpd
    - Database server/application: postgresql, sqlite
    - Programming language interpretor: PHP
    - web browser: firefox
    - email client: evolution
    - graphics application: inkscape
    - messaging application: pidgin
    - network protocol analyzer: wireshark

Build

    1. Copy the CodeAnalyzer directory to the project directory under the LLVM source code directory.
    2. Depending on the type of the LLVM build, e.g. Release+Asserts, modify the path specified by 
    the INSTALLDIR variable in dataflow/Makefile.
    3. Run ‘make’ and ‘make install’ in data flow directory.
    3. Run ‘make’ and ‘make install’ in CodeAnalyzer directory.  

Usage

    CodeAnalyzer can be started with the opt tool of LLVM. In the test directory, there is a shell 
    script runanalyze.sh, which generates LLVM bitcode for a C/C++ source file and then runs ‘opt’ 
    to execute CodeAnalyzer on the LLVM bitcode. It can easily be modified to  analyze any other C/C++
    source file. To facilitate using CodeAnalyzer with a large project that consists of more than one 
    C/C++ source files, CodeAnalyzer comes with a script wrapper for C/C++ compiler such as gcc/g++. 
    By specifying this wrapper as the compiler when configuring a project or changing the order of 
    paths in the PATH environment so that this wrapper will be used in place of the real gcc/g++, all 
    the source files in a project can be analyzed by CodeAnalyzer without modifying its Makefile. 

Configuration

    The path of the output file of CodeAnalyzer is specified with environmental variable ANALYZER_OUTPUT. 
    Note that the output is appended to the file so that CodeAnalyzer can be used to collect information 
    for more than one C/C++ source file of a large project.
    
    CodeAnalyzer also uses a configuration file analyzer.cfg to allow an user to collect more information
    on demand. For example, the configuration file can specify the name of API functions and struct 
    variables used by a program to perform important operations so that CodeAnalyzer will collect more 
    information for calls to these functions and access to these variables. By default, CodeAnalyzer looks 
    for analyzer.cfg in the current directory where it is executing. The path for analyzer.cfg can also be 
    changed via environmental variable ANALYZER_CFG. The format of the configuration file is described in 
    the example analyzer.cfg.
    
Publication

    CodeAnalyzer was used in the research work that proposes a novel technique to mitigate software 
    vulnerabilities, "Talos: Neutralizing Vulnerabilities with Security Workarounds for Rapid Response", 
    which is published in the proceedings of the 37th IEEE Symposium on Security and Privacy (Oakland 2016). 
