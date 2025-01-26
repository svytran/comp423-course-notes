# Setting up a dev container for Rust
* Primary author: [Vy Tran](https://github.com/svytran)
* Reviewer: [Archana Goli](https://github.com/archgoli)

Welcome! In this tutorial, you'll learn how to set up a basic project from scratch in Rust. By the end of this guide, you'll be able to compile and run a Hello COMP423 example. Along the way, you'll also learn how to set up a git repository (git repo) and basic Rust development container (dev container) in Visual Studio Code (VS Code).

!!! info "Why Rust?"
    Rust is a compiled programming language that emphasizes performance, type safety, and concurrency. It enforces memory safety (all references point to valid memory) without a traditional garbage collecter. It is popular for systems programming. 
---
## Prerequisites
Before we get started, make sure you have everything needed for this tutorial:  
1. __A Github Account__: Sign up for [GitHub](https://github.com/) if you don't already have an account.    
2. __Git installed__: Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don't have it already.  
3. __Visual Studio Code (VS Code)__: Download and install it [here](https://code.visualstudio.com/).  
4. __Docker installed__: This is required to run the dev container. Get it [here](https://www.docker.com/products/docker-desktop/).  
5. __Command-line basics__: Review the Learn a CLI text when in doubt.
---

## Part 1 - Project Setup: Creating the Repository
!!! note "Citations"
    Instructions in this section is reused+edited from the 423 [MkDocs tutorial](https://comp423-25s.github.io/resources/MkDocs/tutorial/) by Kris Jordan.  
### Step 1. Create a local directory and initialize git 
(A) Open your terminal or command prompt. 

(B) Create a new directory for your project (Note: If you'd like to reorganize this tutorial somewhere else on your machine, go ahead and change into that parent directory first. By default, this will be in your user's home directory.):
``` py
mkdir comp423-rust-tutorial
cd comp423-rust-tutorial
```
!!! question "What does this mean?"
    * ```mkdir comp423-rust-tutorial```: Creates a new directory called comp423-rust-tutorial.
    * ```cd comp423-rust-tutorial```: Changes into that directory so you can work inside it.
(C) Initalize a new Git repository: 
``` py
git init
```
!!! question "What is the effect of running the init subcommand?"
    Turn this directory into a git repository by running ```git init``` which initializes a folder as a new, empty git repository.
(D) Create a README file:
``` py
echo "# Rust Tutorial" > README.md
git add README.md
git commit -m "Initial commit with README"
```
### Step 2. Create a Remote Repository on GitHub
(1) Log in to your GitHub account and navigate to the [Create a New Repository](https://github.com/new) page.   

(2) Fill in the details as follows:

* __Repository Name:__ ```comp423-rust-tutorial```
* __Description:__ "Rust tutorial for Comp423."
* __Visibility:__ Public  

(3) Do not initialize the repository with a README, .gitignore, or license.

(4) Click __Create Repository__.

### Step 3. Link your Local Repository to GitHub
(1) Add the GitHub repository as remote: 
``` py
git remote add origin https://github.com/<your-username>/comp423-rust-tutorial.git
```
Replace ```<your-username>``` with your GitHub username. 

(2) Check your default branch name with the subcommand ```git branch```. If it's not ```main```, rename it to ```main``` with the following command: ```git branch -M main```. Old versions of ```git``` choose the name ```master``` for the primary branch, but ```main``` is the modern version.   

(3) Push your local commits to the GitHub repository:
``` py
git push --set-upstream origin main
```
!!! info "Understanding the set --upstream Flag"
    * ```git push --set-upstream origin main```: This command pushes the main branch to the remote repository origin. The ```--set-upstream``` flag sets up the main branch to track the remote branch so that future pushes and pulls can be done without specifying the branch name and just writing ```git push origin``` when working on your local ```main``` branch. Use ```-u``` as a short flag.
(4) Back in your web browser, refresh your GitHub repository to see that the same commit you made locally has been pushed to remote. 

---
## Part 2 - Setting up the Development Environment
!!! note "Citations"
    Instructions in this section is reused + edited from the 423 [MkDocs tutorial](https://comp423-25s.github.io/resources/MkDocs/tutorial/) by Kris Jordan. 
### What is a Development (Dev) Container?
A dev container is a Docker-based development environment defined by a configuration file that specifies the tool, runtime, and dependencies needed for a project.
### Why is a Dev Container valuable?
With a dev container, everyone is able to work in an identical environment and have shared experiences. This is extremely useful for teams as it reduces bugs from *"it only works on my machine"* issues.


### Step 1. Add Development Conainer Configuration
1. In VS Code, open the ```comp423-rust-tutorial``` directory. You can do this via: File > Open Folder.  
2. Install the __rust-analyzer__ extension by the Rust Programming Language Group.
3. Install the __Dev Containers__ extension for VS Code if you don't already have it installed.  
4. Create a ```.devcontainer``` directory in the root of your project with the following file inside of this "hidden" configuration directory:

```.devcontainer/devcontainer.json```

The ```devcontainer.json``` file defines the configuration for your development environment. Here, we're specifying the following:

* ```name```: A descriptive name for your dev container.
* ```image```: The Docker image to use, in this case, is the latest version of a Rust environment. [Microsoft has a collection of base images for many programming language environments, but you can also customize your own by providing the path to a Docker file.](https://hub.docker.com/r/microsoft/vscode-devcontainers)
* ```customizations```: A list of VSCode extensions that can help you with your development. In this case, it's the rust-analyzer. When you search for VSCode exntesions on the marketplace, you'll find the string identifier of each extension in its sidebar. Adding extensions here helps ensure that other developers on your project have them installed in their dev containers automatically. 
!!! quote "What is rust-analyzer?"
     Quoted from [GitHub](https://rust-analyzer.github.io/), the rust-analyzer extension provides useful features like code completion and goto definition for many code editors.
``` json
{
    "name": "Rust Tutorial",
    "image": "mcr.microsoft.com/devcontainers/rust:latest",
    "customizations": {
        "vscode": {
            "extensions": [
                "rust-lang.rust-analyzer"
            ]
        }
    }
}
```
### Step 2. Reopen the Project in a VSCode Dev Container
Reopen the project in the container by pressing ```Ctrl+Shift+P``` (or ```Cmd+Shift+P on Mac```), typing "Dev Containers: Reopen in Container" and selecting that option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running ```rustc --version``` to see your dev container is running a recent version of Rust without much effort! (The most recent Rust version is 1.84.0 at the time of writing this in January 2025.)

!!! tip 
    If this doesn't work, check that rustc is installed by running rustc -h. A help message should output to the console for the Rust compiler. 

In addition, run ```cargo --version``` to check that cargo is running.

### What is Cargo?
Cargo is a tool used for Rust projects because of its usefulness in handling a lot of tasks for you -- building your code, downloading the libraries your code depends on, etc -- because of this, a vast majority of Rust project use Cargo. Cargo comes installed with Rust, so if your dev container was able to run a recent version of Rust, you're all set for the next steps to running Hello COMP423!

---
## Part 3 - Compiling and Running Hello World with Rust
### Step 1. Creating a Project with Cargo
First, create a new project using Cargo within your VSCode terminal.
``` py
cargo new hello_comp423 --vcs none
cd hello_comp423
```
!!! question "What does this mean?"
    * ``` cargo new hello_comp423 --vcs none
    ```: ```cargo new``` creates a new Cargo package that has the directory and project named hello_423, and Cargo creates its files in a directory of the same name. The flag ```--vcs none``` stands for Version Control System none which doesn't create a new git repository automatically on your behalf.
    * ``` cd hello_comp423
    ```: This goes into your hello_423 directory and files. You'll see that Cargo has two files and one directory: a Cargo.toml file and a src directory with a main.rs file inside.
Open ```hello_comp423/src/main.rs```. You'll see something like this:
``` rust
fn main() {
    println!("Hello, world!");
}
```
Cargo has generated a "Hello, world!" program for you! All you have to do is change it to "Hello COMP423". It should look like this: 
``` py
fn main() {
    println!("Hello COMP423");
}
```
### Step 2. Building and Running a Cargo Project
From your hello_comp423 directory, build you project by entering the following command in your terminal:
``` 
cargo build
```
When running the command, you should see something similar to this: 
``` 
   Compiling hello_comp423 v0.1.0 (/workspaces/comp423-rust-tutorial/hello_comp423)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.20s
```
This command creates an executable file in target/debug/hello_comp423 instead of your current directory. Since the default build is a debug build, Cargo puts the binary in a directory named debug. You can run the executable with the command
```
./target/debug/hello_comp423
```
This will print Hello COMP423 to the terminal!

### Comparing to ```gcc``` Command
You might remember from Comp211 that you compile a C program using ```gcc``` and then running the generated executable.  
#### Compiling Code
* The command ```cargo build``` in Rust (Cargo) compiles the Rust code (by default in debug mode) and produces an executable binary file located at ```./target/debug/hello_comp423```
* The command ```gcc -o hello_comp423 hello_comp423.c``` in C (gcc) will compile the hello_comp423 file into an executable named hello_comp423. The resulting binary is located in the current directory ```./hello_comp423```
#### Running the compiled binary
* After building, you can run the compiled binary in Rust (Cargo) by doing: ```./target/debug/hello_comp423```. This will run the executable that was generated by ```cargo build```
* After compiling, you can run the executable in C (gcc) by doing: ```./hello_comp423```. This will run the binary that was created by gcc.

### Alternative Subcommand: ```cargo run```
Instead of building a project with ```cargo build``` and running it with ```./target/debug/hello_comp423```, you can also use ```cargo run``` to compile and run the executable in one command.   
Doing so, you should get something similar to this:
```
vscode ➜ /workspaces/comp423-rust-tutorial/hello_comp423 (main) $ cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/hello_comp423`
Hello 423
```
Using ```cargo run``` is much more convenient than having to remember to run ```cargo build``` and then using the whole path to binary. 

!!! question "How is this different from ```build```?"
    * ```cargo build``` compiles the code in the project but doesn't run the resulting executable. It only produces the binary in the target/debug directory. You'll want to use this to check for compilation errors or build the executable without running it immediately.
    * ```cargo run``` combines the function of cargo build and running the compiled binary. It'll compile the project if needed and then immediately run the resulting executable. You'll want to use this if you want to test or debug your program and see its output quickly.

## Congratulations! 
### If you made it to the end, you've successfully learned how to run a dev container for Rust and ran "Hello COMP423".