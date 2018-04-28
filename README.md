# JohntheRipper
## Abstract

Password is one of the most important authentication nowadays. We can meet them every walks of life, for example: ATM, social networks, e-services,... Usually,  passwords are stored as hashcodes rather than plain text in databases, by using some special hashing algorithms, to prevent against thread. 

Password cracking is process of recovering passwords from data that have been stored in the system. A common approach is to try to guess repeatedly for the password and check against an available cryptographic hash of the password. The larger probability password have, the longer time for guessing.GPU is usually used to get faster process because it can operate more guesses per second.

The useful of password cracker: can use to recover a forgotten password, gaining unauthorized access to a system or checking for easily crackable passwords.

In this project, I will introduce one of the best password cracker – John the Ripper,its installation and how to run it on parallel for recover and checking crackable password purposes.
## Principle

### Hashing password
Passwords are stored as hashes rather than plain text in databases by using some special hashing algorithms, to prevent against thread. When a system get a input password, it will automatically transform that password to a set of digits with certain length (called hashes) in some rules so that that every times we input that password, we will get exactly same hash.
Hashing algorithms differ in the way they work and the most notable difference is the length of the number each one spits out.

### Salt 
Salt is a random small piece of text that is added at the beginning of the password before hashing process. The final hash will content a part of salt hash which is stored together with salt text in system. By this way, the hashes would be more complicated to crack.

### Password cracking
Password cracking is the process of recovering passwords from hashes.
Some popular techniques used in Password cracking:
- Dictionary attack: This uses a big dictionary that contain common passwords and their hashes, and try to find a matching hashes pair with checking hash.
- Brute force attack: This works through all possible alpha-numeric combination and try to match their hashes with checking hash.
- Rainbow table attack: A rainbow table is a list of pre-computed hashes - the numerical value of an encrypted password, used by most systems today - and that’s the hashes of all possible password combinations for any given hashing algorithm. This work try to compare those hashes to the checking hashes 

### Strength of password is various
Strength of a password is depend on its length and diversity of character type.
  - A password have 4 characters of number : 10.000 possible passwords.
  - A password have 4 characters of number, upper and lower case : 14.776.336 possible passwords.
  - A password have 4 characters of number, upper and lower case, symbol : 81.450.625 possible passwords.
  - A password have 6 characters of number, upper and lower case, symbol : 735.091.890.625 possible passwords.
  - A password have 8 characters of number, upper and lower case, symbol : 6.634.204.312.890.625 possible passwords.
The more number of characters your password is made up, the larger number of possible passwords.

### Parallel computing in Password cracking
Estimating time for Intel core i5 7th 4x2.5Ghz to cracking password of 8 characters without salt: 184 hours. This work will take a huge time and can effect seriously to your CPU.

Parallel system can do the bigger number of Flops, allow us make several more guesses passwords per second.

Modern graphics processing hardware (GPUs) is very good at hashing which can accelerate the cracking process thousand time compare to the parallel CPU system.

### John the Ripper
Can runs on fifteen different platforms.

Combines a number of password crackers into one package, autodetects password hash types, and includes a customizable cracker.

Can be run against various encryptedpassword formats including several crypt password hash types most commonly found on various Unix versions (based on DES, MD5, or Blowfish), KerberosAFS, and Windows NT/2000/XP/2003 LM hash.

## Prerequisites
- OpenSSL.
- GCC.
- John-1.8.0
- OpenMPI.
- OpenCL.

## Running John Benchmark
### Hardware
2 nodes, each node : 2 x Intel(R) Xeon(R) CPU E5-2660 v2 @ 2.20GHz (20 cores total)
NVIDIA Tesla P100 GPU
### Install and running
```
bash Build_Johntheripper_Rocket
```
### Result
#### For 2 Nodes (20 cores in total)

#### For 4 Nodes (40 core in total)

#### For 2 Nodes (20 cores) and 1 GPU


## Building on small cluster for cracking process
### Hardware

### Installing
Install MPI
```
sudo apt-get install libcr-dev mpich2 mpich2-doc
```

Install OpenCL
```
sudo apt install ocl-icd-opencl-dev
```

Building John the Ripper with OpenSSL
Replace number_of_cores by one of cores you want to running John the Ripper
```
export N=number_of_cores
```

Execute file
```
chmod +x Build_Johntheripper.sh
```

Building Cracking Programe
```
bash Build_Johntheripper.sh
```

### Testing
Go to run directory
```
cd $HOME/Ripper/JohnTheRipper/run
```

Check the efficiency of John on your system by running it in test mode
```
mpirun -np ${N} ./john -test
```

Compare eficiency of system with different cores
```
mpirun -np 2 ./john --test=10 --format=raw-sha1-linkedin
mpirun -np 4 ./john --test=10 --format=raw-sha1-linkedin
```

### Recover a forgotten password
Taking hashing password from system
```
sudo unshadow /etc/passwd /etc/shadow > passfile2.txt
mpirun -np ${N} ./john --wordlist=combo_not.txt passfile2.txt
```

### Checking crackable password
Replace your_password by password you like to check
```
export passwd=your_password
chmod +x Build_Mkpasswd.sh
bash Build_Mkpasswd.sh
```
### Example result
#### For Recover a forgotten password
#### For checking crackable password

