
## Clone the data
```
$ cp -r /cluster/tufts/biocontainers/workshop/Spring2024Container/data .
$ cd data
$ ls
blastdb.faa  easysfs_0.0.1.def  easySFS_examples  input.fasta
```
## Definition file
```
Bootstrap: docker
From: condaforge/mambaforge

%labels
    Author "Yucheng Zhang <yzhang85@tufts.edu>"

%help
    This container contains the 0.0.1 of easySFS.  

%post
    mamba install -c conda-forge spectrum numpy pandas scipy 
    mamba install -c bioconda dadi pandas 
    cd /opt/
    git clone https://github.com/isaacovercast/easySFS.git
    cd easySFS
    chmod +x *.py

%environment
    export PATH=/opt/easySFS:$PATH
```

## Build container
```
$ srun -N1 -n2 -t2:00:00 -p interactive --pty bash 
$ module purge
$ module load apptainer/1.2.5-no-suid
$ apptainer build easysfs_0.0.1.sif easysfs_0.0.1.def
```

## Run the container with exec
```
$ apptainer exec  -B /cluster/tufts easysfs_0.0.1.sif easySFS.py -i easySFS_examples/wcs_1200.vcf -p easySFS_examples/wcs_pops.txt --preview -a
INFO:    fuse2fs not found, will not be able to mount EXT3 filesystems
INFO:    underlay of /etc/localtime required more than 50 (75) bind mounts

  Processing 2 populations - ['nuttalli', 'pugetensis']

    Running preview mode. We will print out the results for # of segregating sites
    for multiple values of projecting down for each population. The dadi
    manual recommends maximizing the # of seg sites for projections, but also
    a balance must be struck between # of seg sites and sample size.

    For each population you should choose the value of the projection that looks
    best and then rerun easySFS with the `--proj` flag.
    
nuttalli
(2, 165)	(3, 248)	(4, 305)	(5, 350)	(6, 387)	(7, 419)	(8, 447)	(9, 472)	(10, 495)	(11, 515)	(12, 533)	(13, 551)	(14, 567)	(15, 581)	(16, 595)	(17, 608)	(18, 620)	(19, 632)	(20, 642)	(21, 653)	(22, 662)	(23, 672)	(24, 680)	(25, 689)	(26, 697)	(27, 704)	(28, 712)	(29, 719)	(30, 725)	(31, 732)	(32, 738)	(33, 744)	(34, 750)	(35, 755)	(36, 761)	(37, 766)	(38, 771)	(39, 775)	(40, 780)	(41, 785)	(42, 789)	(43, 793)	(44, 797)	(45, 800)	(46, 804)	(47, 808)	(48, 811)	(49, 812)	(50, 815)	(51, 807)	(52, 810)	(53, 781)	(54, 784)	(55, 716)	(56, 718)	(57, 626)	(58, 629)	(59, 482)	(60, 484)	(61, 337)	(62, 338)	(63, 203)	(64, 203)	(65, 107)	(66, 108)	(67, 63)	(68, 63)	(69, 44)	(70, 44)	(71, 29)	(72, 29)	(73, 12)	(74, 12)	

pugetensis
(2, 166)	(3, 249)	(4, 308)	(5, 355)	(6, 394)	(7, 428)	(8, 459)	(9, 486)	(10, 510)	(11, 533)	(12, 554)	(13, 573)	(14, 592)	(15, 609)	(16, 625)	(17, 640)	(18, 654)	(19, 668)	(20, 680)	(21, 693)	(22, 704)	(23, 716)	(24, 726)	(25, 736)	(26, 746)	(27, 756)	(28, 765)	(29, 773)	(30, 782)	(31, 790)	(32, 798)	(33, 805)	(34, 812)	(35, 819)	(36, 826)	(37, 833)	(38, 839)	(39, 845)	(40, 851)	(41, 857)	(42, 863)	(43, 868)	(44, 874)	(45, 879)	(46, 884)	(47, 889)	(48, 893)	(49, 898)	(50, 902)	(51, 907)	(52, 911)	(53, 915)	(54, 919)	(55, 923)	(56, 927)	(57, 930)	(58, 934)	(59, 938)	(60, 941)	(61, 944)	(62, 948)	(63, 951)	(64, 954)	(65, 957)	(66, 960)	(67, 963)	(68, 966)	(69, 969)	(70, 971)	(71, 974)	(72, 977)	(73, 979)	(74, 982)	(75, 984)	(76, 986)	(77, 989)	(78, 991)	(79, 993)	(80, 995)	(81, 997)	(82, 1000)	(83, 1002)	(84, 1004)	(85, 1006)	(86, 1007)	(87, 1009)	(88, 1011)	(89, 1013)	(90, 1015)	(91, 1016)	(92, 1018)	(93, 1020)	(94, 1021)	(95, 1023)	(96, 1024)	(97, 1026)	(98, 1027)	(99, 1029)	(100, 1030)	(101, 1032)	(102, 1033)	(103, 1034)	(104, 1036)	(105, 1037)	(106, 1038)	(107, 1040)	(108, 1041)	(109, 1042)	(110, 1043)	(111, 1044)	(112, 1045)	(113, 1046)	(114, 1048)	(115, 1049)	(116, 1050)	(117, 1051)	(118, 1052)	(119, 1053)	(120, 1054)	(121, 1055)	(122, 1055)	(123, 1056)	(124, 1057)	(125, 1058)	(126, 1059)	(127, 1060)	(128, 1061)	(129, 1061)	(130, 1062)	(131, 1063)	(132, 1064)	(133, 1065)	(134, 1065)	(135, 1061)	(136, 1062)	(137, 1055)	(138, 1056)	(139, 1038)	(140, 1038)	(141, 1012)	(142, 1013)	(143, 961)	(144, 962)	(145, 865)	(146, 866)	(147, 760)	(148, 761)	(149, 649)	(150, 649)	(151, 491)	(152, 492)	(153, 384)	(154, 384)	(155, 271)	(156, 271)	(157, 211)	(158, 211)	(159, 158)	(160, 158)	(161, 102)	(162, 102)	(163, 76)	(164, 76)	(165, 61)	(166, 61)	(167, 51)	(168, 51)	(169, 45)	(170, 45)	(171, 39)	(172, 39)	(173, 37)	(174, 37)	(175, 33)	(176, 33)	(177, 30)	(178, 30)	(179, 26)	(180, 26)	(181, 22)	(182, 22)	(183, 18)	(184, 18)	(185, 10)	(186, 10)
```
[Previous: Run container](hands-on/run.md)