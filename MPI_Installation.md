# MPI Installation Instructions

The following instructions assume a Unix-based operating system.
Since each Unix-based system may differ, installation commands can vary across distributions.
However, the general installation process remains the same.

1. Update the system package index  
2. Install the GCC compiler (skip this step if already installed)  
3. Install an MPI implementation  
4. Verify the installation  

> **Note:**  
> MPI implementations are actively maintained and widely used on modern Linux systems.
> However, compatibility issues may arise depending on the specific combination of
> operating system version and MPI distribution.
> 
> If any issues arise, please refer to the accompanying notes document for
> environment-specific observations and troubleshooting details.

### Example: Ubuntu-based systems

```bash
# 1. Update the system package index
sudo apt update

# 2. Install the GCC compiler and build tools (skip if already installed)
sudo apt install -y build-essential


# 3. Install an MPI implementation (OpenMPI)
sudo apt install -y openmpi-bin openmpi-common libopenmpi-dev
```

```bash
# 4. Verify if the C compiler wrapper is installed
mpicc --version
```
<img width="833" height="285" alt="image" src="https://github.com/user-attachments/assets/838bd759-0083-4ab6-80e0-0bc5d9aee914" />

```bash
#5. Verify if the MPI runner is installed
mpiexec --version
```
<img width="927" height="649" alt="image" src="https://github.com/user-attachments/assets/a8c5f337-e60c-4aaa-acf9-851ff82e04f2" />



<br><p align="center">
<strong>✔ Installation verified.</strong><br>
If the commands above produce output similar to the examples shown, the MPI environment has been successfully installed.
</p>

```
It is strongly recommended to validate the installation by compiling and executing a simple MPI program.
Please refer to the Readme file how_to_run.md.
```
