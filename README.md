# VASP-DFT-Tutorial
General outline of how to perform DFT calculation using VASP with some advanced calculation topics as well

# DFT CALCULATIONS TUTORIAL:
## **VASP** (*Vienna Ab-Initio Simulation Package*)
*Texas State University*, Physics Department

1<sup>st</sup> Edition

*by* Eric Welch

**Created**: June, 2016.

**Last Updated**: May the 4th, 2017.


**NB:** All credit for VASP and any post processing programs are due to the authors of these programs.  I only take credit for creating this tutorial in the sense that I condensed a lot of information learned through hours of reading the VASP/other program documentation and many online resources.  I think it would be best if you were to google search VASP documentation and learn the theory as you go …this really is an ongoing learning process.  You will never know it all, but you can always keep striving towards mastery.  Enjoy!!

**Note from the Author:** These tutorials were written after I realized that the VASP tutorials online are too advanced for a beginner to DFT calculations.  They all seem to be written as though they were part of a workshop such that many ambiguities would be covered in said workshop.  Thus, I wanted to compile a list of my knowledge in the year where I’ve had to teach myself most of these types of calculations; I am a theoretician, but my advisor is an experimentalist, so I was mostly self-taught in terms of the calculations.  I’ve tried to cover everything I learned as well as highlight any headaches incurred during that learning process, so that you don’t have to go through the same headaches.  There is value in making mistakes yourself, but I feel that that should come in doing the calculations, and not necessarily in learning how to do them; in other words, I want this to be a tool to get someone acquainted with methodology of doing DFT calculations, and I’d rather they make errors in taking this tool and implementing it into their own work, rather than them have to struggle to even get work started.  Hopefully this will become clear once you’ve traversed this tutorial.  Best of wishes on your ventures into DFT calculations …they are the reason I fell in love with theoretical solid state physics and why I choose to pursue this as a PhD topic in Germany.  You can do so much with the knowledge of ab initio calculations.  Cheers!!

Eric

If you want to get in touch with me in the future, I will maintain a Gmail account at <ericwwelch314@gmail.com> or my old advisors Dr. Zakhidov, Dr. Scolfaro or Dr. Swartz will know how to get in touch with me.

Also, I very much hope that an errata will be started, once this tutorial becomes a circulation within the department (and if we choose to put this online) and this tutorial is updated and corrected for any errors.

**Update**
I wanted to give credit where it is most definitely due:  Thanks to all that helped in this learning process ...Gabe Leitao, Pablo Borges, James Shook, Luisa Scolfaro, Paul Erhart, Amanda Neukirch, Sergei Tretiak, and especially to my PhD advisor Alex Zakhidov.  If I forgot your name, I am sorry, but know that I am eternally grateful for your contribution to my learning.  

## TABLE OF CONTENTS
Chapters:
- I.  	[Command line (Mac/Linux)](#chap1)
- II.  	[Putty (Windows)](#chap2)
- III. 	[Vim text editor](#chap3)
- IV.  	[FileZilla FTP (file transfer protocol) software](#chap4)
- V.  	[VASP submission script](#chap5)
- VI.   [4 input files](#chap6)
- VII.  [Self-consistent run – forces on ions](#chap7)
- VIII. [Non self-consistent run – density of states and dispersion relationship E(k)/band structures → p4vasp and VESTA](#chap8)
- IX.  	[LDA+U correction](#chap9)
- X.  	[Hybrid functionals](#chap10)
- XI.  	[Dielectric constant calculations](#chap11)
- XII.  [Effective mass calculations – parabolic band fitting](#chap12)
- XIII. [Phonon calculations – phonopy](#chap13)
- XIV.  [Bash scripting](#chap14)
- XV.   [Flow chart of a ‘full’ set of ground state calculations for GaAs with plots](#chap15)


<a name="chap1"></a>
## Chapter I: USING COMMAND LINE (MAC/UBUNTU/LINUX)

Command line or Terminal is the DOS prompt, user interface that allows one to navigate between folders, copy/paste files, create directories and more with the use of single or multiple “commands” given in command line “language.”  To access, in Ubuntu or Linux, search the programs folder and find either Command Prompt or Terminal …these are some of the most common used functions and what they do along with an example for each:

**(a) Coping files:**
```bash
cp a /b
```

cp is the copy function that copies file a to folder b
one may copy many things to one folder, in one line e.g cp a b c d e f g/ would copy files a through f to folder g
however, to copy to many folders, each folder requires a separate line (bash scripting helps to alleviate that issue, so look forward to that chapter if you are interested)

**(b) Browsing directories:**
```bash
cd a/
```

cd is the change directories function that moves from the current directory into directory a
cd a/b/c moves one into the subdirectory of c, in the subdirectory of b, in the directory of a and can be done through as many subdirectories as is needed
../ - .. moves one back one directory from the current directory e.g. cd ../../ moves one back two directories from the current directory and so on (as many ../ move you that many folders backwards)
cd by itself takes one back to the home directory of that computer

**(c) Listing directories:**
```bash
ls
```

ls is the list function, that lists all of the files in the current directory

**(d) Moving files:**
```bash
mv a b/
```

mv is the move function that moves file a to folder b
if mv is used as mv a b with b being a new name for file a, then mv is a rename function e.g. mv file1.txt file2.txt renames file1.txt to file2.txt

**(e) Deleting files:**
```bash
rm a
```

rm is the remove function that deletes file a

**(f) Create a new directory:**
```bash
mkdir directory_name
```

makes a new directory in the current working directory called directory_name

**(g) The asterisk:**
```bash
* (asterisk)
```

an asterisk after a file denotes that one wants to do the denoted function to all files with the letters before the asterisk e.g. if there are 50 files all called a1.txt to a50.txt, and one wanted to delete them all, one would type rm a* and all of the files would be deleted

<a name="chap2"></a>
## Chapter II: USING PUTTY (WINDOWS)

Be sure to visit [the main page of Putty](http://www.putty.org). Click [here to download it](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

I always choose putty.exe on windows and it has always functioned well for me
After downloading, open Putty, and access the cluster using the domain name of your cluster (star.tr.txstate.edu, in my case), then a Terminal window should open asking for your user name, which isusually your login name given to you for access to the terminal. Mine was my email address minus the domain extension (the '@txstate.edu' part) You will then be prompted for your adjoining password. Once you’ve accessed the cluster, it is a Linux environment, so all of the commands/functions from [Chapter I](#chap1) are the same in this terminal environment.
 
<a name="chap3"></a>
## Chapter III: VIM TEXT EDITOR

(https://en.wikipedia.org/wiki/Vim_(text_editor) , https://en.wikipedia.org/wiki/Vi) 

Vim is a text editor that I prefer to use for the required input files in VASP.  It is usually preinstalled on macs and it is on the cluster.  To access the program, open a terminal/putty window and type, on the command, vim program_name.extension or vim filename.extension, where you can choose any program/file name with any appropriate extension.  In VASP, however, one only must type the name of the input file e.g. vim INCAR to open/create that file.  To open a file with vim, simply type vim and the name of that file, and it will open in the text editor window.

To type in vim, type a lower case i, and then start typing.  Depending on what you are typing, vim may or may not highlight your text if you are creating a bash script or python code.  If you are creating a bash script, the extension is .sh and a python code is .py.  Those are other topics on their own, so I extend the challenge to you to go and learn python as it is extremely powerful for scientific post-process computing; chapter XIV will give a brief outline and an example script for bash scripting.  

To exit vim, first hit esc, then type one of the following commands:
	:x to save the file with the name you gave it originally
	:x new_name to save it to a new name
	:q! to quit without saving

There are plenty of other text editors, and many debates online about which may be the best, so I leave it up to you to choose.  But if you do choose vim, then this chapter should at least get you started using vim.  I also think emacs is a decent text editor if vim isn’t available for some reason.  

<a name="chap4"></a>
## Chapter IV: FILEZILLA (SFTP SECURE FILE TRANSFER PROTOCOL PROGRAM)

FileZilla is an ftp program that is free to download from (https://filezilla-project.org); download the client.  Again, as with vim, there are many ftp software programs, I just happened to choose this one and so I will give a brief outline of how to use the program.  

Once you download the client and install FileZilla, open the program.  In the top left of the open screen, click on the icon that says Open the Site Manager.  On the left of the popup, click the New Site button and call it STAR.  Make sure the host name is star.tr.txstate.edu, protocol is sftp, login is normal, and then input your user name and password.  Click connect and you should be connected to the star cluster.  

The purpose of an ftp (sftp – secure ftp) is to allow one to move files between a physical computer and the “remote” cluster.  In other words, one can move files from the cluster to their personal computer and vice versa. 

<a name="chap5"></a>
## Chapter V: SUBMISSION SCRIPT

The submission script is written in a FORTRAN 90 code and is called `run_vasp535.job`.  Here is the code in raw form, line by line.  Following is an explanation for each line in the code.  

```bash
#!/bin/bash
#
#$ -cwd
#$ -j n
#$ -S /bin/bash
#$ -pe orte 144
#$ -q blades.q
#$ -N SC 
#$ -R yes
#$ -M eww9@txstate.edu
#$ -m ase
#

. /etc/profile.d/modules.sh

module load intel/13.0.1
module load intelmpi/13.0.1
module load atlas/3.8.4

cat $PE_HOSTFILE | awk '{print $1" slots="$2}' > machines

mpirun -np $NSLOTS  ./vaspw20
```

- (1) “shebang” symbol: calls to the bash folder to utilize bash functionality …don’t change
- (2) comment line: no need to change
- (3) don’t change
- (4) don’t change
- (5) don’t change
- (6) calls to the orte line in the cluster and allots 144 cores to each calculation …one wants to change the number to the square of their NPAR tag in the adjoining `INCAR` file (more on this in the input file chapter)
- (7) puts the job on the blades.q node. This may be changed to the other nodes on the cluster by asking for access from the cluster manager (while this is being written, that person is Richard Carney <rcarney@txstate.edu>). There is a longrun queue that has no time limit on a single job, and others that may also be accessed that this author never needed to access.  Also, on a side note, if you have a large number of jobs and need to submit them all, then Richard can create a side queue (I had one called phys.q) for a short period of time so that you don’t bog down any of the other queues.  Only change blades.q to longrun.q or the other queue names. Don’t change –q or the intro symbols.  
- (8) One may change SC to any job name they’d like …only change SC
- (9) don’t change
- (10) can change email address (happens to be the address of the author) to your own. This will have the cluster email you when the job completes …it will be an email from root …only change the email address.
- (11 – 20) don’t change
- (20) if you chose to run vasp from your home directory, you will have to change ./vaspw20 to that file in your home directory.  Currently, this code is written such that a copy of vaspw20 is in the working directory.  

Using your favorite text editor (I always preferred vim as it was preinstalled in Ubuntu and Mac), copy and paste the above script in its entirety, and ensure the first line did not get cutoff as it’s very important that the first line is complete, as it is the line that calls to the bash script library. Save the file as `run_vasp535.job`, and convert it to a full executable in the Terminal by typing (in the working directory) `chmod 777 run_vasp535.job`.  This should turn the text of the `run_vasp535.job` green (won’t necessarily, but does on a lot of computers) meaning you can now type qsub run_vasp535.job to send your job to the cluster (after completing the next sections on input files of course …sending it to the cluster with only the submission script is rather meaningless).  

Once you create this file, you will need to get login access to the STAR cluster as well as a copy of `vaspw20` from Dr. Luisa Scolfaro (or Dr. Alex Zakhidov, or contact the current cluster manager). `vaspw20` is the actual VASP script. I put the submission script as well as the `vaspw20` script into every directory from which I plan to submit a job; that way no strings are broken when trying to call `vaspw20` from another directory if the executable is in the working directory (if the code isn’t running for some reason, type into the Terminal, `chmod 777 vaspw20`).

<a name="chap6"></a>
## Chapter VI: 4 input files

### (1) INCAR:
This file is written in FORTRAN 90 and is compressed of all of the input tags that tell vasp how to solve the Kohn-Sham equations as well as compute many ground state and molecular dynamics properties.  The webpage that should and will become one of your best friends for input tags for the `INCAR` file is  (http://cms.mpi.univie.ac.at/wiki/index.php/Category:INCAR).  There are other tags, but most of the important tags are here on this webpage.  The format of the INCAR is as follows:

```
	# Electronic Relaxation
	ISMEAR 	= 0
   	SIGMA 	= 0.01
	
	# Ionic Relaxation
	IBRION 	= 2
	NSW 	= 100
	EDIFFG 	= -0.02

	# Cluster information
	NPAR 	= 6
```

#### Line-by-line explanation:
- Each # line is a comment and can be removed if one wishes. The following descriptions are enumerated as if there were no comment lines or white spaces between input tags:
* `ISMEAR` sets the orbital partial occupancies; 0 is for Gaussian smearing ([http://cms.mpi.univie.ac.at/vasp/vasp/Finite_temperature_approaches_smearing_methods.html](see manual explanation)) 
* `SIGMA` is the width in eV of the smearing
* `IBRION` is the type of ionic relaxation method; 2 is the conjugate gradient method
* `NSW` is the number of attempted Ionic steps. Attempted meaning that the calculation may not need as many steps as you allot; however, `NSW` is the maximum number of ionic steps.
* `EDIFFG` has 2 meanings. Look at the incar link to understand the meanings. In this case, `EDIFFG` is the ionic force convergence cutoff since it is negative, and is set to 2meV/.
* `NPAR` is the tag that should be set to the square root of the value on line 6 of the submission script …this tells the cluster how many cores per kpoint (parallelization)

Remember, this is only a very basic `INCAR` that will allow you to complete a self-consistent run.  More on that in the next section.
	
### (2) KPOINTS:
This file is most likely going to have one of three forms depending on if the calculation is self-consistent or not.  

- **(a) Self-consistent KPOINTS file**: the first time a self-consistent calculation is done, an automatic mesh is used to sample the Brillouin zone (BZ). As KPOINT space is the reciprocal of real space, the two spaces should be inversely proportional i.e. if you have a large supercell, you may only need one point in the BZ. The mesh file should look like this:

```		
  Auto
	0
	MP
	8 8 8
	0 0 0
```

#### Line-by-line explanation:
- Line 1: Tells the code to create an automatic mesh of equally spaced kpoints to sample in solving the KS equations
- Line 2: You are specifying 0 points explicitly, so the code will randomize the kpoints through the BZ
- Line 3: Using the Monkhorst-Pack mesh scheme ([http://cms.mpi.univie.ac.at/vasp/vasp/Automatic_k_mesh_generation.html](explanation here)).
- Line 4: Using an 8x8x8 mesh to sample the BZ, there is a code in the utilities folder that allows one to loop through many values of BZ size (kx, ky, kz) to find the appropriate sized BZ mesh. However, I have found that for a 1x1x1 unit cell, 8x8x8 to 11x11x11 are adequate to converge the electrons and ions into their ground states and give suitable charge densities (CHGCAR).  If you have a large super cell, usually 3x3x3 or larger, then you may only need a 1x1x1 MP mesh, so intead of 8 8 8, use 1 1 1.
- Line 5: The offset from the origin. Consult the [http://cms.mpi.univie.ac.at/vasp/vasp/Automatic_k_mesh_generation.html](link in step 3) for a better understanding of this line.

After a self-consistent run, a file called `IBZKPT` is created with an explicit KPOINTS list that one may rename (rm KPOINTS, mv IBZKPT KPOINTS) and use for further relaxations and other calculations of similar crystal structures.  Using this file allows for far greater efficiency in time for future self-consistent calculations of similar structures.  

- **(b) Non-self-consistent calculations**: after a self-consistent run, the converged CHGCAR and CONTCAR files are copied with the INCAR and POTCAR file into a new folder.  Along with these files, a new KPOINTS file must be created that signifies points and paths of high symmetry in the first BZ.  This requires the use of a correct path of KPOINTS which adequately samples the first BZ along the paths of high symmetry.  There are many resources online, but I actually like the Wikipedia page on the subject (https://en.wikipedia.org/wiki/Brillouin_zone ) because it is straightforward and to the point about the symmetry points/paths for each type of lattice as well as good visuals to help see the most appropriate symmetry paths.  This link gives each of the first BZ as well as a link to the values you’ll need for each of the points of high symmetry i.e. the values for kx, ky and kz for each kpoint.  Here is a KPOINTS file for a non-self-consistent calculation for GaAs in its zinc blende structure form:

```
GaAs	
  40
  line
  rec
	0.50 0.50 0.50
	0.25 0.25 0.25

	0.25 0.25 0.25
	0.00 0.00 0.00

	0.00 0.00 0.00
	0.00 0.25 0.25

	0.00 0.25 0.25
	0.00 0.50 0.50

	0.00 0.50 0.50
	0.25 0.50 0.25

	0.25 0.50 0.25
	0.00 0.00 0.00

```
#### Line-by-line explanation:

- Line 1: Comment line
- Line 2: 40 points between each point of high symmetry
- Line 3: connect each point with a line
- Line 4: use reciprocal points (as opposed to Cartesian points). Rec is usually favored as Cartesian requires one to calculate 2π/a for each kpoint
- Lines 5-21: k-points of high symmetry. Notice well, that one must specify the first point, then the second, skip a line, and start with the second and then the third kpoint, skip a line, third and then fourth kpoint and so on. This is how the code is written so that each new set of kpoints starts with the previous set’s last k-point.  
	
### (3) POSCAR:
This file specifies the crystal basis as well as the lattice constant and primitive lattice vectors.  Here is a sample `POSCAR` for GaAs:

```
GaAs
   1.0
     5.6535    0.0000   0.0000
     0.0000    5.6535   0.0000
     0.0000    0.0000   5.6535
   Ga  As
   4    4
Direct
  0.00 0.00  0.00
  0.00  0.50  0.50
  0.50  0.50  0.00
  0.50  0.00  0.50
  0.75  0.25  0.75
  0.25  0.25  0.25
  0.25  0.75  0.75
  0.75  0.75  0.75
```

#### Line by line:
- (1) Comment line: molecule name
- (2) Can either be the lattice constant, or if the lattice vectors are each different, the value is set to 1.0 and the lattice is defined explicitly in the next 3 lines
- (3-5) Each of the three primitive lattice vectors that define the crystal structure.  Here, this is a cubic structure.
- (4) Atomic species.  One must put at least one space between each atom.  These must be in the same order as the POTCAR file.  
- (5) Number of each type of atom separated by at least one space
- (6) Direct or Cartesian coordinates for the atomic basis (next several lines).
- (7-14) Position of each atom; first the 4 Ga, then the 4 As.  By choosing direct on line 6, one may use the traditional basis values as opposed to having to determine all of the values in terms of the lattice constant.  
			

### (4) POTCAR:
This file is a concenated file containing all of the information about the psuedopotentials to be used in your calculation. Currently (2016), there are GGA (PBE) and LDA (CA) potentials available. The GGA (generalized gradient approximation) (PBE = Perdew Burke Ernzerhof) potentials are located in the `POTCAR/potcar_PBE` folder and the LDA (local density approximation) (CA = Ceperly Alder exchange correlation) are located in the `POTCAR/potcar_LDA` folder.  One must determine which to use before the calculation, and this source helps: <https://www.vasp.at/vasp-workshop/slides/pseudoppdatabase.pdf>.  To create the POTCAR file, one must type in the Terminal the following (replacing the generic names with your specific folder names):

```bash
cat ~/Vasp_home_folder_name/POTCAR/potcar_PBE/Ga/POTCAR ~/Vasp_home_folder_name/POTCAR/potcar_PBE/As/POTCAR > POTCAR
```

It is important that one ensures that the order or POTCAR files is the same as the order of atoms in the POSCAR file.

<a name="chap7"></a>
## Chapter VII: Self-consistent run 

Make sure `INCAR`, `KPOINTS`, `POSCAR`, `POTCAR`, `run_vasp535.job` and `vaspw20` are in the working directory (I usually name it SC), and type into the Terminal, `qsub run_vasp535.job`.  This sends the job to the cluster.  One may type qstat to see if the job has started, qstat –g c gives a more robust view of all of the queues on the cluster, and tail OUTCAR tells you the 20 most recent lines printed to the OUTCAR file (or the last 20 lines of the file if the job has completed).  The .o and .e files give the results of what are finally printed in the `OSZICAR` file and errors in the calculation.  
After the self consistent run, go to the very bottom of the OUTCAR file and find the lines that say FORCES on ions, and ensure the forces are either 0 or below EDIFFG.  If they are not, copy the `INCAR`, `KPOINTS`, `CONTCAR`, `POTCAR`, `WAVECAR`, `vaspw20` and `run_vasp535.job` into a new directory, `mv CONTCAR POSCAR`, and rerun a full self consistent run with the pre converged wave functions (`WAVECAR`) to expedite the initialization process.  Check the new `OUTCAR` and repeat the process until the FORCES on ions are correct for each ion.


<a name="chap8"></a>
## Chapter VIII: Non self-consistent run 
Once the FORCES are correct, copy the `CHGCAR`, `INCAR`, `CONTCAR`, `POTCAR`, `vaspw20` and `run_vasp535.job` into a new directory (I usually call it DB), and create the `KPOINTS` file of high symmetry; here is a sample `KPOINTS` file for GaAs:

```
GaAs 
40
line
rec
0.50 0.50 0.50
0.00 0.25 0.00

0.00 0.25 0.00
0.00 0.00 0.00

0.00 0.00 0.00
0.00 0.25 0.25

0.00 0.25 0.25
0.00 0.50 0.50
```

#### Line-by-line explanation:
- Line 1: Comment line, I usually put the molecule name here
- Line 2: 40 = number of points between each k point of high symmetry 
- Line 3: line = line-mode connects each k point with a line
- Line 4: rec = reciprocal mode …k points are in reciprocal space as opposed to real space where one would have to compute 2*pi/(lattice constant)(kx + ky + kz)  for each k point …I usually use rec since it is easier to find k point first Brillouin zones online in rec values …honestly, it is just a personal preference
- Lines 5-x: 
kpoint 1
kpoint 2

kpoint 2
kpoint 3

kpoint 3 
kpoint 4

...and so on

The numbering scheme is simply the way that VASP computes the eigenvalues.  Just ensure to start with the first k point, then the second, skip a line, start with the second, then the third, skip a line, and so on.  

`mv CONTCAR POSCAR`, remove the ionic lines from the `INCAR` (IBRION, NSW, EDIFFG) and add the line `ICHARG = 11` to the `INCAR` file.  

With the converged `CHGCAR`, 4 inputs and 2 submission files in the working directory, submit the job.  

There will only be one set of electronic calculations done using the converged charge densities.  The most important output file from the non-self-consistent calculation is the `vasprun.xml` file.  This file can be read and parsed using p4vasp which is a Python environment, open source program found here (http://www.p4vasp.at). I would suggest using this on either Ubuntu or Windows in its source code form as the binaries are a headache to install if you aren’t Linux/Cygwin(Windows Linux enviorment) savvy.  p4vasp allows one to plot electron band structures, density of states both partial and total, dielectric functions and much more; it is a very valuable tool, but can be finicky in the first few times of use.  

During the writing of this tutorial, the author met with a fellow graduate student named James Shook who was working on a Matlab code to plot many of the things that p4vasp can plot, but with a much higher resolution and an easier script to manipulate.  If he finishes this, I would suggest getting in touch with him or whomever controls this tutorial to see if they have that Matlab code.  Just a suggestion.  

Also, the final relaxed structure is given in the `CONTCAR` file.  One may visualize this, add tetrahedral cages, look at charge densities (with the `CHGCAR` file) and many other things using VESTA.  This is an open source, crystal structure visualization tool found here (http://jp-minerals.org/vesta/en/).  

I also found this post-processing program helpful in visualizing charge densities (http://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD).  

<a name="chap9"></a>
## Chapter IX: LDA+U correction

Systems that have excited charge carriers who may interact with the ionic lattice need an energy correction term in the form of a Hubbard (U) correction.  This may be done using the the LDA+U method, whereby a Hubbard correction is added to the atomic orbitals of the ionized atoms.  

To determine the U parameter, one must first do a normal self-consistent, non-self-consistent calculation to determine the electronic band gap.  With the band gap know, one then runs many identical system calculations only varying the U parameter until the correct band gap is achieved.  

The tags added to the `INCAR` file are:
LDAU = True
LDAUTYPE = 2 (Dudarev approach)
LDAUU = U parameter: number for every atom  0 for atoms without correction, U in eV (from optimization calculations) for atom with correction
LDAUJ = J parameter: same numbering scheme as U parameter
LASPH = True
LMAXMIX = 4

These are values for the molecule on which I worked.  You will have to determine the values you need if you are doing any type of excited state calculations.  Here is the outline on the theory for the method as well as links to each of the tags: http://cms.mpi.univie.ac.at/wiki/index.php/LDAUTYPE.  

This is a method that works well for studying polarons in systems.

<a name="chap10"></a>
## Chapter X: HYBRID FUNCTIONALS

Hybrid functionals are added to a calculation after a full self consistent run, as an added correction/improvement to the normal functionals.  After a self consistent run, copy `CONTCAR`, `INCAR`, `KPOINTS`, `POTCAR`, `run_vasp535.job` and `vaspw20` into a new directory.  Add the following lines to the INCAR file as well as ([http://cms.mpi.univie.ac.at/wiki/index.php/Specific_hybrid_functionals](reading this link) and it’s references to decide if you need hybrid functionals. If so, type:

```
LHFCALC = .TRUE.
GGA = (whichever GGA you require http://cms.mpi.univie.ac.at/wiki/index.php/GGA).  
HFSCREEN = (the value that corresponds to your type of hybrid functionals)
```

Make sure to read the VASP page on the hybrid functionals to determine which you need.  

**NB**: NPAR must equal 1 as this calculation cannot be done in smaller, parallel pieces.  I would suggest contacting the cluster manager to see if you can be added to the longrun queue as this job may take upwards of 7-10 days.

<a name="chap11"></a>
## Chapter XI: DIELECTRIC CONSTANTS

VASP allows one to compute both the static dielectric tensor and/or the real and imaginary parts of the frequency dependent dielectric function.

A tutorial on how to calculate this:

Coming soon...

<a name="chap12"></a>
## Chapter XII: EFFECTIVE MASSES – PARABOLIC BAND FITTING

Coming soon...

<a name="chap13"></a>
## Chapter XIII: PHONON CALCULATIONS – PHONOPY

Coming soon...

<a name="chap14"></a>
## Chapter XIV: BASH SCRIPTING 

Starting out, it would be a good idea to create a simple bash script called clean.sh that removes all of the output files in a directory, as this is a common thing that you will do over time.  Type vim clean.sh into your terminal (if you are using vim that is).  Type i to start input functionality, and create the following file:

```bash
#!/bin/bash
#
rm filename1 filename2
```

put this into your root/home directory (the one that you go to when you type cd in your terminal by itself), so that when you want to call it in any directory, you can type ~/clean.sh.  to make this file executable, type chmod 777 clean.sh in the directory where clean.sh exists.

A more advanced example:

Lets say you wish to create 100 folders, move 6 files from the working directory into each of those folders, rename a file and submit a job to the cluster.  First, you need to create your 6 input files (for this lets make it easy and use the VASP inputs …so INCAR, KPOINTS, POSCAR, POTCAR, run_vasp535.job, vaspw20) or move them into the working directory.  Ensure your inputs are all correct (we are assuming that there are actually 100 different POSCAR files, each numbered POSCAR_001 to POSCAR_100 …something that may happen if you get to phonon calculations) and type vim workflow.sh into a Terminal.  Then type the following bash script:

```bash
#!/bin/bash
#
for a in {1..100}
do
	mkdir $a
	cp INCAR KPOINTS POSCAR_$a POTCAR run_vasp535.job vaspw20 $a
	cd $a
	mv POSCAR* POSCAR
	qsub run_vasp535.job
	cd ../
done
```

Try to follow through each line and imagine what happens over each for loop …a directory named 1 is created, the input with the proper POSCAR are copied into the directory, one moves into 1, renames the POSCAR file to the correct name for submission, submits the job to the cluster and then moves out of the folder 1 so that the process can repeat for folder 2 and so on.  
	
I promise that learning how to bash script early in your DFT calculation career will save you a lot of time in the future.  Automating tasks helps to prevent redundant tasks that take up time you could otherwise be using to understand the theory and work on more and more calculations, instead of typing mkdir 400 times.  It will be a great tool to have for more than just these calculations, but any type of things you may do in Terminal/Putty.
