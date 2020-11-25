   
What is a Superblock, Inode, Dentry and a File?
===============================================

   Stolen from a Stack Overflow post.

   First and foremost, and I realize that it was not one of the terms from your
question, you must understand metadata. Succinctly, and stolen from Wikipedia,
metadata is data about data. That is to say that metadata contains information
about a piece of data. For example, if I own a car then I have a set of
information about the car but which is not part of the car itself. Information
such as the registration number, make, model, year of manufacture, insurance
information, and so on. All of that information is collectively referred to as
the metadata. In Linux and UNIX file systems metadata exists at multiple levels
of organization as you will see.

   The superblock is essentially file system metadata and defines the file
system type, size, status, and information about other metadata structures
(metadata of metadata). The superblock is very critical to the file system and
therefore is stored in multiple redundant copies for each file system. The
superblock is a very "high level" metadata structure for the file system. For
example, if the superblock of a partition, /var, becomes corrupt then the file
system in question (/var) cannot be mounted by the operating system. Commonly
in this event, you need to run fsck which will automatically select an
alternate, backup copy of the superblock and attempt to recover the file
system. The backup copies themselves are stored in block groups spread through
the file system with the first stored at a 1 block offset from the start of the
partition. This is important in the event that a manual recovery is necessary.
You may view information about ext2/ext3/ext4 superblock backups with the
command dumpe2fs /dev/foo | grep -i superblock which is useful in the event of
a manual recovery attempt. Let us suppose that the dumpe2fs command outputs the
line Backup superblock at 163840, Group descriptors at 163841-163841. We can
use this information, and additional knowledge about the file system structure,
to attempt to use this superblock backup: /sbin/fsck.ext3 -b 163840 -B 1024
/dev/foo. Please note that I have assumed a block size of 1024 bytes for this
example.

   An inode exists in, or on, a file system and represents metadata about a
file. For clarity, all objects in a Linux or UNIX system are files; actual
files, directories, devices, and so on. Please note that, among the metadata
contained in an inode, there is no file name as humans think of it, this will
be important later. An inode contains essentially information about ownership
(user, group), access mode (read, write, execute permissions) and file type.

   A dentry is the glue that holds inodes and files together by relating inode
numbers to file names. Dentries also play a role in directory caching which,
ideally, keeps the most frequently used files on-hand for faster access. File
system traversal is another aspect of the dentry as it maintains a relationship
between directories and their files. Each dentry maps an inode number to a file
name and a parent directory.

   A file, in addition to being what humans typically think of when presented
with the word, is really just a block of logically related arbitrary data.
Comparatively very dull considering all of the work done (above) to keep track
of them.
