# updatedb

Creates or updates a database for [locate](07-locate.md).

It is usually run daily from a `systemd.timer`.

The database file is binary.

It returns with exit status `0` on success, `1` on error.

Its config file is `/etc/updatedb.conf` and the database file is `/var/lib/plocate/plocate.db`.

```bash
$ cat /etc/updatedb.conf
PRUNE_BIND_MOUNTS="yes"
# PRUNENAMES=".git .bzr .hg .svn"
PRUNEPATHS="/tmp /var/spool /media /var/lib/os-prober /var/lib/ceph /home/.ecryptfs /var/lib/schroot"
PRUNEFS="NFS afs autofs binfmt_misc ceph cgroup cgroup2 cifs coda configfs curlftpfs debugfs devfs devpts devtmpfs ecryptfs ftpfs fuse.ceph fuse.cryfs fuse.encfs fuse.glusterfs fuse.gocryptfs fuse.gvfsd-fuse fuse.mfs fuse.rclone fuse.rozofs fuse.sshfs fusectl fusesmb hugetlbfs iso9660 lustre lustre_lite mfs mqueue ncpfs nfs nfs4 ocfs ocfs2 proc pstore rpc_pipefs securityfs shfs smbfs sysfs tmpfs tracefs udev udf usbfs"
```

| Variable | Description |
| --- | --- |
| PRUNEPATHS | A whitespace-separated list of path names of directories which should not be scanned. |
| PRUNENAMES | A whitespace-separated list of directory names (without paths) which should not be scanned. |
| PRUNEFS | A whitespace-separated list of file system types (as used in /etc/mtab) which should not be scanned. The file system type matching is case-insensitive. |
| PRUNE_BIND_MOUNTS | One of the strings 0, no, 1 or yes. If it is 1 or yes, bind mounts are not scanned. All file systems mounted in the subtree of a bind mount are skipped as well, even if they are not bind mounts. As an exception, bind mounts of a directory on itself are not skipped. |

| Short | Long | Description |
| --- | --- | --- |
| -o FILE | --output FILE | Write the database to FILE instead of using the default database. |
| -U PATH | --database-root PATH | Store only results of scanning the file system subtree rooted at PATH to the generated database. |
| -v | --verbose | Output path names of files to standard output. |
| -e PATHS | --add-prunepaths PATHS | Add entries in white-space-separated list PATHS to PRUNEPATHS. |
| -n NAMES | --add-prunenames NAMES | Add entries in white-space-separated list NAMES to PRUNENAMES. |
| -f FS | --add-prunefs FS | Add entries in white-space-separated list FS to PRUNEFS. |
|  | --prunepaths PATHS | Set PRUNEPATHS to PATHS, overriding the configuration file. |
|  | --prunenames NAMES | Set PRUNENAMES to NAMES, overriding the configuration file. |
|  | --prunefs FS | Set PRUNEFS to FS, overriding the configuration file. |
|  | --prune-bind-mounts FLAG | Set PRUNE_BIND_MOUNTS to FLAG, overriding the configuration file. |
|  | --debug-pruning | Write a summary of the available options to standard output and exit successfully. |
| -l FLAG | --require-visibility FLAG | Set the "require file visibility before reporting it" flag in the generated database to FLAG.<br>If FLAG is 0 or no, or if the database file is readable by "others" or it is not owned by plocate, locate outputs the database entries even if the user running locate could not have read the directory necessary to find out the file described by the database entry.<br>If FLAG is 1 or yes (the default), locate checks the permissions of parent directories of each entry before reporting it to the invoking user. To make the file existence truly hidden from other users, the database group is set to plocate and the database permissions prohibit reading the database by users using other means than locate, which is set-gid plocate.<br>Note that the visibility flag is checked only if the database is owned by plocate and it is not readable by "others". |

Some examples:

```bash
$ updatedb
/var/lib/plocate/: Permission denied
$ sudo updatedb
$ sudo updatedb -U /home
$ updatedb -U ~/Projects/ -o ~/projects.db
fchowm: Operation not permitted
$ sudo updatedb -U ~/Projects -o ~/projects.db
$ updatedb -U ~/Projects/ -o ~/projects.db -l 0
$ updatedb --prunepaths="/tmp /var/spool"
```
