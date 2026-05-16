# utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation
Altair slc python script to run openradioss tensile strength simulation
%let pgm=utl-altair-slc-openradioss-tensile-strength-simulation-in-python-no-gui-no-mouse-surfing;

%stop_submission;

Altair slc openradioss tensile strength simulation python no gui no mouse surfing

Too long to post here see github
[github](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation)

[annimation](https://www.youtube.com/watch?v=5S3gGxW89yU)
www.youtube.com/watch?v=5S3gGxW89yU

A complete self-contained reproducible example is presented that is entirely scripted, no
GUI or mouse surfing requeried.

SOURCE
[openradioss simulation](//openradioss.atlassian.net/wiki/spaces/OPENRADIOSS/pages/24444938/Tensile+Test+Example+Tutorial+Using+Gmsh)

[First Frame](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/frame_0001.png)

[Last Frame](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/frame_0041.png)

For validation an exact reproduction of the published plot was created,
[Published](https://openradioss.atlassian.net/wiki/spaces/OPENRADIOSS/pages/24444938/Tensile+Test+Example+Tutorial+Using+Gmsh)
[validation](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/Fixed_End_RBODY_FX_X_FORCE.png)

Graphical Analysis - See below for Summary Tables

[Stress-strain curve         ](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/stress_strain.png)
[Force vs Displacement       ](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/force_displace.png)
[Energy - Early Phase        ](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/energy_evolution.png
[Energy - Full Simulation    ](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/energy_evolutionfull.png)
[X-Momentum Primary Direction](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/xmomentumevolve.png)
[Time Step Evolution         ](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/timestephistory.png)
[Energy Balance              ](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/energybalance.png)
[Fraction Plastic Deformation](https://github.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/blob/main/plasticwork.png)

CONTENTS

 1 Preparation
 1 Get github inputs
 2 Get animation & time history
 3 Time history table
 4 Csv to history table
 5 Validation plot
 6 Staistical summary
 7 Many plots
    Stress-strain curve for DP600 steel tensile test
    Force vs Displacement - Raw Tensile Test Data
    Energy Evolution - Early Phase (First 1 Time Unit)
    Energy Evolution - Full Simulation (0-0.018 Time Units)
    X-Momentum Evolution Primary Direction Impulse
    Time Step Evolution Solver Adaptation to Simulation Complexity
    Energy Balance Verification Plot
    Fraction of Internal Energy from Plastic Deformation
 8 Animation to vtk file
 9 Vtk to mp4

Related Repos
https://github.com/rogerjdeangelis/utl-altair-slc-post-processing-radioss-files-using-openradioss
https://github.com/rogerjdeangelis/utl-personal-altair-slc-with-matlab-sympy-and-r-finite-element-cold-plate-heat-transfer


/*
 _                                        _   _
/ |  _ __  _ __ ___ _ __   __ _ _ __ __ _| |_(_) ___  _ __
| | | `_ \| `__/ _ \ `_ \ / _` | `__/ _` | __| |/ _ \| `_ \
| | | |_) | | |  __/ |_) | (_| | | | (_| | |_| | (_) | | | |
|_| | .__/|_|  \___| .__/ \__,_|_|  \__,_|\__|_|\___/|_| |_|
    |_|            |_|
*/

libname workx "d:/wpswrkx";

MOST OF THE INSTALLS ARE ONLY NEEDED FOR THE ANIMATION, YOU MAY NOT NEED STEPS II-V IF
YOU ARE NOT INTERESTED DIN THE ANIMATION. INTEL API IS NEEDED FOR CUSTOMIZATION OF PENRADIOSS
OR PARALLEL PROCESSING?


I INSTALL OPENRADIOSS
---------------------

 a  Go to https://github.com/OpenRadioss/OpenRadioss/releases
 b  Download openradioss_win64.zip
 c  Create directory c:/openradioss
 d  From the unzipped file copy all folders, see below, to c:/openradioss
    The result should look like

    c:/openradioss (should look like)

      <DIR>   exec
      <DIR>   extlib
      <DIR>   hm_cfg_files
      <DIR>   licenses
      <DIR>   openradioss_gui


II  INSTALL INTEL OPENAPI TOOLKIT YOU NEED TO INSTALL VERSION 2 FROM THE ARCHIVES
-----------------------------------------------------------------------------

 a  https://www.intel.com/content/www/us/en/developer/articles/tool/oneapi-archive.html
 b  It automaticall installs at C:/Program Files (x86)/Intel/oneapi


III INSTALL VISUAL STUDIO
-------------------------

 a  https://visualstudio.microsoft.com/vs/community/
 b  C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools


IV  INSTALL FFMPEG
------------------

 a  https://www.gyan.dev/ffmpeg/builds/
 b  C:\Program Files\ffmpeg
 c  esential version
 d  unzip and save to c:/program files


V   INSTALL PARAVIEW
--------------------

 a  https://www.paraview.org/download/
 b  It will install at C:\Program Files\ParaView-6.1.0-Windows-Python3.12-msvc2017-AMD64


VI  Download SLC macros and place in your autocall library for instance c:/wpsoto (also in this repo)
-----------------------------------------------------------------------------------------------------
 a  https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories
 b  utlfkil.sas
 d  slc_pvbegin  /*-- runs a virtual python ---*/
 e  slc_pvend
 f  slc_psbegin  /*--- runs powershell      ---*/
 g  slc_psend



VII YOU NEED TO PUT THIS IN YOUR AUTOEXEC
-----------------------------------------

  a libname workx "d:/wpswrkx";
  b or set in betewen each section below
    (many many sections. I suggest you put it in your autoexec ---*/


/*---
 ____              _          _ _   _           _      _                   _
|___ \   __ _  ___| |_   __ _(_) |_| |__  _   _| |__  (_)_ __  _ __  _   _| |_ ___
  __) | / _` |/ _ \ __| / _` | | __| `_ \| | | | `_ \ | | `_ \| `_ \| | | | __/ __|
 / __/ | (_| |  __/ |_ | (_| | | |_| | | | |_| | |_) || | | | | |_) | |_| | |_\__ \
|_____| \__, |\___|\__| \__, |_|\__|_| |_|\__,_|_.__/ |_|_| |_| .__/ \__,_|\__|___/
        |___/           |___/                                 |_|

You do not need to run this. You can manually  create d:/rad and download the rad files from this repp

What powershell is doing ( you can do the following manually)

  1 deletes d:/rad directory if it exists
  2 recreate empty d:/rad
  3 copy files from github
    D:\rad\gmsh_tensile_LAW36_BIQUAD_0000.rad
    D:\rad\gmsh_tensile_LAW36_BIQUAD_0001.rad
    D:\rad\tensile_test_coupon.inc

---*/

libname workx "d:/wpswrkx";  /*--- put in autoexec ---*/

proc datasets lib=workx kill;
run;

%slc_psbegin; /*--- call powershell ---*/
cards4;
# Deletes d:/rad and all subdirectories/files, recreates the folder, then
# downloads the three OpenRadioss input files from your GitHub repository.

$targetDir = "D:\rad"

# 1. Remove the directory and everything inside it (forcefully, recursively)
if (Test-Path $targetDir) {
    Write-Host "Removing existing directory: $targetDir" -ForegroundColor Yellow
    Remove-Item -Path $targetDir -Recurse -Force
}


# 2. Recreate the empty directory
Write-Host "Creating fresh directory: $targetDir" -ForegroundColor Yellow
New-Item -Path $targetDir -ItemType Directory -Force | Out-Null

# 3. Define the source files (GitHub raw URLs) and their destination names
$files = @(
    @{
        Source = "https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/gmsh_tensile_LAW36_BIQUAD_0000.rad"
        Dest   = "D:\rad\gmsh_tensile_LAW36_BIQUAD_0000.rad"
    },
    @{
        Source = "https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/gmsh_tensile_LAW36_BIQUAD_0001.rad"
        Dest   = "D:\rad\gmsh_tensile_LAW36_BIQUAD_0001.rad"
    },
    @{
        Source = "https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/tensile_test_coupon.inc"
        Dest   = "D:\rad\tensile_test_coupon.inc"
    }
)

# 4. Download each file using Invoke-WebRequest
Write-Host "Downloading files to $targetDir ..." -ForegroundColor Yellow
foreach ($file in $files) {
    try {
        Write-Host "  Downloading: $($file.Source)" -ForegroundColor Cyan
        Invoke-WebRequest -Uri $file.Source -OutFile $file.Dest
        Write-Host "    Saved to: $($file.Dest)" -ForegroundColor Green
    }
    catch {
        Write-Host "    ERROR: Failed to download $($file.Source)" -ForegroundColor Red
        Write-Host "    Exception: $($_.Exception.Message)" -ForegroundColor Red
    }
}

# 5. Optional: List the contents of D:\rad to verify
Write-Host "`nContents of $targetDir :" -ForegroundColor Yellow
Get-ChildItem -Path $targetDir | Format-Table Name, Length -AutoSize

Write-Host "`nScript completed." -ForegroundColor Green
;;;;
%slc_psend;

/*           _               _
  ___  _   _| |_ _ __  _   _| |_
 / _ \| | | | __| `_ \| | | | __|
| (_) | |_| | |_| |_) | |_| | |_
 \___/ \__,_|\__| .__/ \__,_|\__|
                |_|
*/

/**************************************************************************************************************************/
/*                                                                                                                        */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*  DIRECTORY OF D:\RAD                                                                                                   */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*                                                                                                                        */
/*    Contents of D:\rad :                                                                                                */
/*                                                                                                                        */
/*    Name                               Length                                                                           */
/*    ----                               ------                                                                           */
/*    gmsh_tensile_LAW36_BIQUAD_0000.rad  10494                                                                           */
/*    gmsh_tensile_LAW36_BIQUAD_0001.rad    280                                                                           */
/*    tensile_test_coupon.inc             69984                                                                           */
/*                                                                                                                        */
/*    Script completed.                                                                                                   */
/*                                                                                                                        */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*  GMSH_TENSILE_LAW36_BIQUAD_0001.RAD (0.018/.0002 x 91 animation frames)                                                */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*                                                                                                                        */
/*  /ANIM/DT                                                                                                              */
/*  0 0.001                                                                                                               */
/*  /ANIM/SHELL/TENS/STRESS/ALL                                                                                           */
/*  /ANIM/SHELL/TENS/STRAIN/ALL                                                                                           */
/*  /PRINT/-500/55                                                                                                        */
/*  # Emax Mmax Nmax NTH NANIM NERR_POSIT                                                                                 */
/*  0 0 0 1 1 0                                                                                                           */
/*  /TFILE/0                                                                                                              */
/*  #            dT_HIS                                                                                                   */
/*  0.000010                                                                                                              */
/*  /VERS/2022                                                                                                            */
/*  /DT/NODA/CST/0                                                                                                        */
/*  0.9 0 0                                                                                                               */
/*  /RUN/gmsh_tensile_LAW36_BIQUAD/1/                                                                                     */
/*                  0.04                                                                                                  */
/*                                                                                                                        */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*  GMSH_TENSILE_LAW36_BIQUAD_0000.RAD                                                                                    */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*                                                                                                                        */
/*     # Copyright (C) 2022 Altair Engineering Inc. ("Holder")                                                            */
/*     # Model is licensed by Holder under CC BY-NC 4.0                                                                   */
/*     # (https://creativecommons.org/licenses/by-nc/4.0/legalcode).                                                      */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /BEGIN                                                                                                             */
/*     gmsh_tensile_LAW36_BIQUAD                                                                                          */
/*           2022         0                                                                                               */
/*                       Mg                  mm                   s                                                       */
/*                       Mg                  mm                   s                                                       */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     #-  1. CONTROL CARDS:                                                                                              */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /TITLE                                                                                                             */
/*                                                                                                                        */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     #include tensile_test_coupon.inc                                                                                   */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     #-  2. MATERIALS:                                                                                                  */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /MAT/PLAS_TAB/2                                                                                                    */
/*     DP600 from SSAB Homepage                                                                                           */
/*     #              RHO_I                                                                                               */
/*                   7.8E-9                   0                                                                           */
/*     #                  E                  Nu           Eps_p_max               Eps_t               Eps_m               */
/*                   210000                  .3                   0                   0                   0               */
/*     #  N_funct  F_smooth              C_hard               F_cut               Eps_f                  VP               */
/*              1         0                   0                   0                   0                   0               */
/*     #  fct_IDp              Fscale   Fct_IDE                EInf                  CE                                   */
/*              0                   0         0                   0                   0                                   */
/*     # func_ID1  func_ID2  func_ID3  func_ID4  func_ID5                                                                 */
/*             14                                                                                                         */
/*     #           Fscale_1            Fscale_2            Fscale_3            Fscale_4            Fscale_5               */
/*                        1                                                                                               */
/*     #          Eps_dot_1           Eps_dot_2           Eps_dot_3           Eps_dot_4           Eps_dot_5               */
/*     ...                                                                                                                */
/*     ...                                                                                                                */
/*     ---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|                */
/*     #- 10. TIME HISTORIES:                                                                                             */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /TH/NODE/2                                                                                                         */
/*     TH_Measuring_Nodes                                                                                                 */
/*     #     var1      var2      var3      var4      var5      var6      var7      var8      var9     var10               */
/*     DEF                                                                                                                */
/*     #    NODid     Iskew                                           NODname                                             */
/*        1000002         0                                                                                               */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /TH/RBODY/3                                                                                                        */
/*     TH RBODY                                                                                                           */
/*     #     var1      var2      var3      var4      var5      var6      var7      var8      var9     var10               */
/*     DEF                                                                                                                */
/*     #     Obj1      Obj2      Obj3      Obj4      Obj5      Obj6      Obj7      Obj8      Obj9     Obj10               */
/*              1         2                                                                                               */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /END                                                                                                               */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*                                                                                                                        */
/*                                                                                                                        */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*  TENSILE_TEST_COUPON.INC (NODE GEPMETRY?)                                                                              */
/*  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*/
/*                                                                                                                        */
/*     #RADIOSS STARTER                                                                                                   */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     # Created by Gmsh, Radioss Mesh Interface by PaulAltair sharp@altair.com                                           */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /BEGIN                                                                                                             */
/*     tensile_test_coupon.inc                                                                                            */
/*           2022         0                                                                                               */
/*                       Mg                  mm                   s                                                       */
/*                       Mg                  mm                   s                                                       */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     #-  1. NODES:                                                                                                      */
/*     #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|               */
/*     /NODE                                                                                                              */
/*              1                 175                  11                   0                                             */
/*              2              162.31                6.25                   0                                             */
/*              3              160.37                6.06                   0                                             */
/*              4              158.42                   6                   0                                             */
/*              5                 100               5.975                   0                                             */
/*              6               41.58                   6                   0                                             */
/*              7               39.63                6.06                   0                                             */
/*              8               37.69                6.25                   0                                             */
/*              9                  25                  11                   0                                             */
/*             10        -3.55271E-15                  11                   0                                             */
/*             11        -3.55271E-15                 -11                   0                                             */
/*             12                  25                 -11                   0                                             */
/*             13               37.69               -6.25                   0                                             */
/*             14               39.63               -6.06                   0                                             */
/*             15               41.58                  -6                   0                                             */
/*             16                 100              -5.975                   0                                             */
/*             17              158.42                  -6                   0                                             */
/*     ....                                                                                                               */
/*     ...                                                                                                                */
/*           669        61       577       576        62                                                                  */
/*           670         9       578       577        61                                                                  */
/*           671        63       579       578         9                                                                  */
/*           672        67       580       579        63                                                                  */
/*           673        64       581       580        67                                                                  */
/*           674        68       582       581        64                                                                  */
/*           675        65       583       582        68                                                                  */
/*           676        69       584       583        65                                                                  */
/*           677        66       585       584        69                                                                  */
/*           678        10        70       585        66                                                                  */
/*    #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|                */
/*    #enddata                                                                                                            */
/*    /END                                                                                                                */
/*    #---1----|----2----|----3----|----4----|----5----|----6----|----7----|----8----|----9----|---10----|                */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*                   _     _
(_)_ __  _ __  _   _| |_  | | ___   __ _
| | `_ \| `_ \| | | | __| | |/ _ \ / _` |
| | | | | |_) | |_| | |_  | | (_) | (_| |
|_|_| |_| .__/ \__,_|\__| |_|\___/ \__, |
        |_|                        |___/
*/
1                                          Altair SLC            10:06 Sunday, May 10, 2026

NOTE: Copyright 2002-2025 World Programming, an Altair Company
NOTE: Altair SLC 2026 (05.26.01.00.000758)
      Licensed to Roger DeAngelis
NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
NOTE: AUTOEXEC source line
1       +  ï»¿ods _all_ close;

NOTE: AUTOEXEC processing completed

1          %slc_psbegin;
2         cards4;

NOTE: The file 'c:\temp\ps_pgm.ps1' is:
      Filename='c:\temp\ps_pgm.ps1',
      Owner Name=SLC\suzie,
      File size (bytes)=0,
      Create Time=14:40:04 Mar 21 2026,
      Last Accessed=10:06:41 May 10 2026,
      Last Modified=10:06:41 May 10 2026,
      Lrecl=32767, Recfm=V

NOTE: 51 records were written to file 'c:\temp\ps_pgm.ps1'
      The minimum record length was 80
      The maximum record length was 195
NOTE: The data step took :
      real time : 0.000
      cpu time  : 0.015


3         # Deletes d:/rad and all subdirectories/files, recreates the folder, then
4         # downloads the three OpenRadioss input files from your GitHub repository.
5
6         $targetDir = "D:\rad"
7
8         # 1. Remove the directory and everything inside it (forcefully, recursively)
9         if (Test-Path $targetDir) {
10            Write-Host "Removing existing directory: $targetDir" -ForegroundColor Yellow
11            Remove-Item -Path $targetDir -Recurse -Force
12        }
13
14
15        # 2. Recreate the empty directory
16        Write-Host "Creating fresh directory: $targetDir" -ForegroundColor Yellow
17        New-Item -Path $targetDir -ItemType Directory -Force | Out-Null
18
19        # 3. Define the source files (GitHub raw URLs) and their destination names
20        $files = @(
21            @{
22                Source = "https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/gmsh_tensile_LAW36_BIQUAD_0000.rad"
23                Dest   = "D:\rad\gmsh_tensile_LAW36_BIQUAD_0000.rad"
24            },
25            @{
26                Source = "https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/gmsh_tensile_LAW36_BIQUAD_0001.rad"
27                Dest   = "D:\rad\gmsh_tensile_LAW36_BIQUAD_0001.rad"
28            },
29            @{
30                Source = "https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/tensile_test_coupon.inc"
31                Dest   = "D:\rad\tensile_test_coupon.inc"
32            }
33        )
34
35        # 4. Download each file using Invoke-WebRequest
36        Write-Host "Downloading files to $targetDir ..." -ForegroundColor Yellow
37        foreach ($file in $files) {
38            try {
39                Write-Host "  Downloading: $($file.Source)" -ForegroundColor Cyan
40                Invoke-WebRequest -Uri $file.Source -OutFile $file.Dest
41                Write-Host "    Saved to: $($file.Dest)" -ForegroundColor Green
42            }
43            catch {
44                Write-Host "    ERROR: Failed to download $($file.Source)" -ForegroundColor Red
45                Write-Host "    Exception: $($_.Exception.Message)" -ForegroundColor Red
46            }
47        }
48
49        # 5. Optional: List the contents of D:\rad to verify
50        Write-Host "`nContents of $targetDir :" -ForegroundColor Yellow
51        Get-ChildItem -Path $targetDir | Format-Table Name, Length -AutoSize
52
53        Write-Host "`nScript completed." -ForegroundColor Green
54        ;;;;
55        %slc_psend;

NOTE: The infile rut is:
      Unnamed Pipe Access Device,
      Process=powershell.exe -executionpolicy bypass -file c:/temp/ps_pgm.ps1 >  c:/temp/ps_pgm.log,
      Lrecl=32756, Recfm=V

NOTE: No records were written to file PRINT

NOTE: No records were read from file rut
Stderr output:
Remove-Item : Cannot remove item D:\rad: The process cannot access the file 'D:\rad' because it is being used by
another process.
At C:\temp\ps_pgm.ps1:9 char:5
+     Remove-Item -Path $targetDir -Recurse -Force
+     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : WriteError: (D:\rad:DirectoryInfo) [Remove-Item], IOException
    + FullyQualifiedErrorId : RemoveFileSystemItemIOError,Microsoft.PowerShell.Commands.RemoveItemCommand
NOTE: The data step took :
      real time : 2.977
      cpu time  : 0.015



NOTE: The infile rut is:
      Unnamed Pipe Access Device,
      Process=powershell.exe -executionpolicy bypass -file c:/temp/ps_pgm.ps1 >  c:/temp/ps_pgm.log,
      Lrecl=32767, Recfm=V

NOTE: No records were written to file PRINT

NOTE: No records were read from file rut
Stderr output:
Remove-Item : Cannot remove item D:\rad: The process cannot access the file 'D:\rad' because it is being used by
another process.
At C:\temp\ps_pgm.ps1:9 char:5
+     Remove-Item -Path $targetDir -Recurse -Force
+     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : WriteError: (D:\rad:DirectoryInfo) [Remove-Item], IOException
    + FullyQualifiedErrorId : RemoveFileSystemItemIOError,Microsoft.PowerShell.Commands.RemoveItemCommand
NOTE: The data step took :
      real time : 1.275
      cpu time  : 0.015



NOTE: The infile 'c:\temp\ps_pgm.log' is:
      Filename='c:\temp\ps_pgm.log',
      Owner Name=SLC\suzie,
      File size (bytes)=1085,
      Create Time=10:30:46 Mar 22 2026,
      Last Accessed=10:06:45 May 10 2026,
      Last Modified=10:06:45 May 10 2026,
      Lrecl=32767, Recfm=V

Removing existing directory: D:\rad
Creating fresh directory: D:\rad
Downloading files to D:\rad ...
  Downloading: https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/gmsh_tensile_LAW36_BIQUAD_0000.rad
    Saved to: D:\rad\gmsh_tensile_LAW36_BIQUAD_0000.rad
  Downloading: https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/gmsh_tensile_LAW36_BIQUAD_0001.rad
    Saved to: D:\rad\gmsh_tensile_LAW36_BIQUAD_0001.rad
  Downloading: https://raw.githubusercontent.com/rogerjdeangelis/utl-altair-slc-python-script-to-run-openradioss-tensile-strength-simulation/refs/heads/main/tensile_test_coupon.inc
    Saved to: D:\rad\tensile_test_coupon.inc

Contents of D:\rad :

Name                               Length
----                               ------
gmsh_tensile_LAW36_BIQUAD_0000.rad  10494
gmsh_tensile_LAW36_BIQUAD_0001.rad    909
tensile_test_coupon.inc             69984



Script completed.
NOTE: 21 records were read from file 'c:\temp\ps_pgm.log'
      The minimum record length was 0
      The maximum record length was 191
NOTE: 21 records were written to file PRINT

NOTE: The data step took :
      real time : 0.015
      cpu time  : 0.000


ERROR: Error printed on page 1

NOTE: Submitted statements took :
      real time : 4.634
      cpu time  : 0.125

/*____             _                _                 _   _               ___    _   _                                _
|___ /   __ _  ___| |_   __ _ _ __ (_)_ __ ___   __ _| |_(_) ___  _ __   ( _ )  | |_(_)_ __ ___   ___   ___  ___ _ __(_) ___  ___
  |_ \  / _` |/ _ \ __| / _` | `_ \| | `_ ` _ \ / _` | __| |/ _ \| `_ \  / _ \/\| __| | `_ ` _ \ / _ \ / __|/ _ \ `__| |/ _ \/ __|
 ___) || (_| |  __/ |_ | (_| | | | | | | | | | | (_| | |_| | (_) | | | || (_>  <| |_| | | | | | |  __/ \__ \  __/ |  | |  __/\__ \
|____/  \__, |\___|\__| \__,_|_| |_|_|_| |_| |_|\__,_|\__|_|\___/|_| |_| \___/\/ \__|_|_| |_| |_|\___| |___/\___|_|  |_|\___||___/
        |___/
*/

/*--- ONLY USE THIS DATASTEP IF YOU NEED TO CUSTOMIZE THE GMSH_TENSILE_LAW36_BIQUAD_0001.RAD  ---*/
/*--- YOU DO NOT NEED TO RUN THIS DATASTEP FOR THIS EXAMPLE                                   ---*/

/*--- UNCOMMENT IF YOU NEED TO CUSTOMIZE
data _null_;
 file "d:/rad/gmsh_tensile_LAW36_BIQUAD_0001.rad";
 input;
 _infile_=trim(_infile_);
 put _infile_;
 putlog _infile_;
cards4;
/ANIM/DT
0 0.001
/ANIM/SHELL/TENS/STRESS/ALL
/ANIM/SHELL/TENS/STRAIN/ALL
/PRINT/-500/55
# Emax Mmax Nmax NTH NANIM NERR_POSIT
0 0 0 1 1 0
/TFILE/0
#            dT_HIS
0.000010
/VERS/2022
/DT/NODA/CST/0
0.9 0 0
/RUN/gmsh_tensile_LAW36_BIQUAD/1/
                0.04
;;;;
run;
---*/


/*--- START HERE ---*/
/*--- START HERE ---*/

/*--- CREATE SIEMEN ANIMATION AND TIME HISTORY CSV FILE  ---*/

options validvarname=v7;
options set=PYTHONHOME "D:\py314";
proc python;
submit;
import os
import pandas as pd
import subprocess
from pathlib import Path

# ============================================================================
# CONFIGURATION
# ============================================================================

OPENRADIOSS_PATH = Path("C:/openradioss")
STARTER_EXE = OPENRADIOSS_PATH / "exec" / "starter_win64.exe"
ENGINE_EXE = OPENRADIOSS_PATH / "exec" / "engine_win64.exe"
TH_TO_CSV_EXE = OPENRADIOSS_PATH / "exec" / "th_to_csv_win64.exe"

MODEL_DIR = Path("D:/rad")
STARTER_FILE = "gmsh_tensile_LAW36_BIQUAD_0000.rad"
ENGINE_FILE = "gmsh_tensile_LAW36_BIQUAD_0001.rad"

# Time history file (output from simulation)
TH_FILE = MODEL_DIR / "gmsh_tensile_LAW36_BIQUADT01"
# CSV output file
CSV_FILE = MODEL_DIR / "results.csv"

SETVARS_PATH = Path("C:/Program Files (x86)/Intel/oneAPI/setvars.bat")
OMP_NUM_THREADS = "1"
KMP_STACKSIZE = "400m"

# ============================================================================
# ENVIRONMENT SETUP
# ============================================================================

def setup_environment():
    env = os.environ.copy()
    env["OPENRADIOSS_PATH"] = str(OPENRADIOSS_PATH)
    env["RAD_CFG_PATH"] = str(OPENRADIOSS_PATH / "hm_cfg_files")
    env["RAD_H3D_PATH"] = str(OPENRADIOSS_PATH / "extlib" / "h3d" / "lib" / "win64")
    env["OMP_NUM_THREADS"] = OMP_NUM_THREADS
    env["KMP_STACKSIZE"] = KMP_STACKSIZE

    hm_reader = OPENRADIOSS_PATH / "extlib" / "hm_reader" / "win64"
    if hm_reader.exists():
        env["PATH"] = str(hm_reader) + ";" + env.get("PATH", "")

    # Add tools directory to PATH for th_to_csv
    tools_dir = OPENRADIOSS_PATH / "tools"
    if tools_dir.exists():
        env["PATH"] = str(tools_dir) + ";" + env.get("PATH", "")

    return env

def run_cmd(cmd, cwd, env, log_file):
    """Run command and capture output to log file"""
    with open(log_file, 'w') as f:
        result = subprocess.run(cmd, shell=True, cwd=cwd, env=env,
                                stdout=f, stderr=subprocess.STDOUT, text=True)
    return result.returncode

def convert_th_to_csv(th_file, csv_file, env):
    """Convert time history binary file to CSV format"""
    if not th_file.exists():
        print(f"Warning: Time history file not found: {th_file}")
        return False

    if not TH_TO_CSV_EXE.exists():
        print(f"Warning: th_to_csv_win64.exe not found at {TH_TO_CSV_EXE}")
        return False

    print(f"Converting time history: {th_file.name} -> {csv_file.name}")

    # Run th_to_csv and redirect output to CSV file
    cmd = f'"{TH_TO_CSV_EXE}" "{th_file}"'

    try:
        with open(csv_file, 'w') as f:
            result = subprocess.run(cmd, shell=True, cwd=str(MODEL_DIR), env=env,
                                    stdout=f, stderr=subprocess.PIPE, text=True)

        if result.returncode == 0 and csv_file.exists() and csv_file.stat().st_size > 0:
            # Count lines to verify data
            with open(csv_file, 'r') as f:
                line_count = sum(1 for _ in f)
            print(f"  [OK] Created {csv_file.name} ({line_count} lines, {csv_file.stat().st_size / 1024:.1f} KB)")

            # Show first few lines as preview
            with open(csv_file, 'r') as f:
                header = f.readline().strip()
                first_data = f.readline().strip() if line_count > 1 else ""
            print(f"  Header: {header[:100]}...")
            return True
        else:
            print(f"  [FAILED] Conversion failed (exit code {result.returncode})")
            if result.stderr:
                print(f"    Error: {result.stderr[:200]}")
            return False
    except Exception as e:
        print(f"  [ERROR] {e}")
        return False

# ============================================================================
# MAIN EXECUTION
# ============================================================================

def main():
    print("=" * 60)
    print("OpenRadioss Tensile Test Simulation")
    print("=" * 60)

    # Quick verification
    if not STARTER_EXE.exists() or not ENGINE_EXE.exists():
        print("ERROR: OpenRadioss executables not found")
        return 1

    env = setup_environment()
    setvars = f'call "{SETVARS_PATH}" intel64 vs2022 > nul 2>&1 && ' if SETVARS_PATH.exists() else ""

    # Run Starter
    print("\n[1/3] Running Starter...")
    starter_input = MODEL_DIR / STARTER_FILE
    rc = run_cmd(f'{setvars}"{STARTER_EXE}" -i "{starter_input}"',
                  str(MODEL_DIR), env, MODEL_DIR / "starter.log")
    if rc != 0:
        print(f"Starter failed (exit {rc}). Check starter.log")
        return rc

    # Run Engine
    print("[2/3] Running Engine...")
    engine_input = MODEL_DIR / ENGINE_FILE
    rc = run_cmd(f'{setvars}"{ENGINE_EXE}" -i "{engine_input}"',
                  str(MODEL_DIR), env, MODEL_DIR / "engine.log")
    if rc != 0:
        print(f"Engine failed (exit {rc}). Check engine.log")
        return rc

    # List animation files created
    anim_files = list(MODEL_DIR.glob("*.A0*")) + list(MODEL_DIR.glob("*.h3d"))
    if anim_files:
        print(f"\nAnimation files created ({len(anim_files)}):")
        for f in sorted(anim_files):
            size_kb = f.stat().st_size / 1024
            print(f"  {f.name} ({size_kb:.1f} KB)")
    else:
        print("\nWarning: No animation files found")

    # Convert time history to CSV
    print("\n[3/3] Converting time history to CSV...")
    convert_th_to_csv(TH_FILE, CSV_FILE, env)

    # Summary
    print("\n" + "=" * 60)
    print("SIMULATION COMPLETE")
    print("=" * 60)
    print(f"Results saved to: {MODEL_DIR}")
    print(f"  - Time history CSV: {CSV_FILE.name}")
    print(f"  - Starter log: starter.log")
    print(f"  - Engine log: engine.log")
    if anim_files:
        print(f"  - Animation files: {len(anim_files)} files")

    # Verify CSV was created
    if CSV_FILE.exists():
        print(f"\nCSV file size: {CSV_FILE.stat().st_size / 1024:.1f} KB")
        print("You can now open results.csv in Excel or Python for analysis.")

    print("\nDone.")
    return 0

if __name__ == "__main__":
    exit(main());
endsubmit;
run;

/*           _               _
  ___  _   _| |_ _ __  _   _| |_
 / _ \| | | | __| `_ \| | | | __|
| (_) | |_| | |_| |_) | |_| | |_
 \___/ \__,_|\__| .__/ \__,_|\__|
                |_|
*/

/*************************************************************************************************************************/
/* ============================================================                                                          */
/* OpenRadioss Tensile Test Simulation                                                                                   */
/* ============================================================                                                          */
/*                                                                                                                       */
/* [1/3] Running Starter...                                                                                              */
/* [2/3] Running Engine...                                                                                               */
/*                                                                                                                       */
/* Warning: No animation files found                                                                                     */
/*                                                                                                                       */
/* [3/3] Converting time history to CSV...                                                                               */
/* Converting time history: gmsh_tensile_LAW36_BIQUADT01 -> results.csv                                                  */
/*   [OK] Created results.csv (6 lines, 0.2 KB)                                                                          */
/*                                                                                                                       */
/*   Header: ...                                                                                                         */
/*                                                                                                                       */
/* ============================================================                                                          */
/* SIMULATION COMPLETE                                                                                                   */
/* ============================================================                                                          */
/*                                                                                                                       */
/* Results saved to: D:\rad                                                                                              */
/*                                                                                                                       */
/*   - Time history CSV: results.csv                                                                                     */
/*   - Starter log: starter.log                                                                                          */
/*   - Engine log: engine.log                                                                                            */
/*                                                                                                                       */
/*                                                                                                                       */
/* CSV file size: 0.2 KB                                                                                                 */
/* You can now open results.csv in Excel or Python for analysis.                                                         */
/*                                                                                                                       */
/* Done.                                                                                                                 */
/*                                                                                                                       */
/* ======================================================================================================================*/
/* DIRECTORY OF D:\RAD                                                                                                   */
/* ======================================================================================================================*/
/*                                                                                                                       */
/* Directory of D:\rad                                                                                                   */
/*                                                                                                                       */
/* gmsh_tensile_LAW36_BIQUAD_0000.rad                  10,494  INPUT FILES                                               */
/* gmsh_tensile_LAW36_BIQUAD_0001.rad                     911                                                            */
/* tensile_test_coupon.inc                             69,984                                                            */
/*                                                                                                                       */
/* starter.log                                          3,577                                                            */
/* engine.log                                          35,142                                                            */
/*                                                                                                                       */
/* gmsh_tensile_LAW36_BIQUADA001                      113,325  ANIMATION FILES                                           */
/* gmsh_tensile_LAW36_BIQUADA002                      113,325                                                            */
/* gmsh_tensile_LAW36_BIQUADA003                      113,325                                                            */
/*                                                                                                                       */
/* gmsh_tensile_LAW36_BIQUADA039                      113,325                                                            */
/* gmsh_tensile_LAW36_BIQUADA040                      113,325                                                            */
/* gmsh_tensile_LAW36_BIQUADA041                      113,325                                                            */
/*                                                                                                                       */
/* gmsh_tensile_LAW36_BIQUADT01                       880,980                                                            */
/* gmsh_tensile_LAW36_BIQUADT01.csv                 2,483,695  OUTPUT CSV                                                */
/* results.csv                                            158                                                            */
/*                                                                                                                       */
/* gmsh_tensile_LAW36_BIQUAD_0000.out                  79,643                                                            */
/* gmsh_tensile_LAW36_BIQUAD_0000_0001.rst          1,046,547                                                            */
/* gmsh_tensile_LAW36_BIQUAD_0001.out                  43,547                                                            */
/* gmsh_tensile_LAW36_BIQUAD_0001_0001.rst          1,046,867                                                            */
/*                                                                                                                       */
/* 104 File(s)                                                                                                           */
/*                                                                                                                       */
/* ======================================================================================================================*/
/* STARTER LOG                                                                                                           */
/* ======================================================================================================================*/
/*                                                                                                                       */
/*  ************************************************************************                                             */
/*  **                        OpenRadioss Starter                         **                                             */
/*  **            Non-linear Finite Element Analysis Software             **                                             */
/*  **                                                                    **                                             */
/*  **                  Windows 64 bits, Intel compiler                   **                                             */
/*  **                      Double Precision Version                      **                                             */
/*  ************************************************************************                                             */
/*  ** OpenRadioss Software                                               **                                             */
/*  ** COPYRIGHT (C) 1986-2026 Altair Engineering, Inc.                   **                                             */
/*  ** Licensed under GNU Affero General Public License.                  **                                             */
/*  ** See License file.                                                  **                                             */
/*  ************************************************************************                                             */
/*                                                                                                                       */
/*   .. UNITS SYSTEM                                                                                                     */
/*   .. CONTROL VARIABLES                                                                                                */
/*   .. STARTER RUNNING ON    1 THREAD                                                                                   */
/*   .. FUNCTIONS & TABLES                                                                                               */
/*   .. MATERIALS                                                                                                        */
/*   .. NODES                                                                                                            */
/*   .. SUBMODELS                                                                                                        */
/*   .. PROPERTIES                                                                                                       */
/*   .. 3D SHELL ELEMENTS                                                                                                */
/*   .. SUBSETS                                                                                                          */
/*   .. BOX                                                                                                              */
/*   .. ELEMENT GROUPS                                                                                                   */
/*   .. PART GROUPS                                                                                                      */
/*   .. NODE GROUP                                                                                                       */
/*   .. BOUNDARY CONDITIONS                                                                                              */
/*   .. IMPOSED VELOCITIES                                                                                               */
/*   .. DOMAIN DECOMPOSITION                                                                                             */
/*   .. ELEMENT GROUPS                                                                                                   */
/*   .. RIGID BODIES                                                                                                     */
/*   .. ELEMENT BUFFER INITIALIZATION                                                                                    */
/*   .. GEOMETRY PLOT FILE                                                                                               */
/*   .. PARALLEL RESTART FILES GENERATION                                                                                */
/*                                                                                                                       */
/* -----------------------------------------------------                                                                 */
/*                    ** COMPUTE TIME INFORMATION **                                                                     */
/*                                                                                                                       */
/*  EXECUTION STARTED      :      2026/06/01  13:07:31                                                                   */
/*  EXECUTION COMPLETED    :      2026/06/01  13:07:40                                                                   */
/*                                                                                                                       */
/*  ELAPSED TIME...........=           9.09 s (actaullt about 10 seconds)                                                */
/*                                 00.00.09                                                                              */
/* -----------------------------------------------------                                                                 */
/*                                                                                                                       */
/*      NORMAL TERMINATION                                                                                               */
/*      ------------------                                                                                               */
/*               0 ERROR(S)                                                                                              */
/*               0 WARNING(S)                                                                                            */
/*                                                                                                                       */
/* PLEASE CHECK LISTING FILE FOR FURTHER DETAILS                                                                         */
/*                                                                                                                       */
/*                                                                                                                       */
/* ======================================================================================================================*/
/* ENGINE LOG                                                                                                            */
/* ======================================================================================================================*/
/*                                                                                                                       */
/*   ************************************************************************                                            */
/*   **                         OpenRadioss Engine                         **                                            */
/*   **            Non-linear Finite Element Analysis Software             **                                            */
/*   **                                                                    **                                            */
/*   **                  Windows 64 bits, Intel compiler                   **                                            */
/*   **                      Double Precision Version                      **                                            */
/*   ************************************************************************                                            */
/*   ** OpenRadioss Software                                               **                                            */
/*   ** COPYRIGHT (C) 1986-2026 Altair Engineering, Inc.                   **                                            */
/*   ** Licensed under GNU Affero General Public License.                  **                                            */
/*   ** See License file.                                                  **                                            */
/*   ************************************************************************                                            */
/*                                                                                                                       */
/*    ROOT: gmsh_tensile_LAW36_BIQUAD  RESTART: 0001                                                                     */
/*    NUMBER OF HMPP PROCESSES     1                                                                                     */
/*    01/06/2022                                                                                                         */
/*    NC=       0 T= 0.0000E+00 DT= 1.7912E-07 ERR=  0.0% DM/M= 0.0000E+00                                               */
/*        ANIMATION FILE: gmsh_tensile_LAW36_BIQUADA001 WRITTEN                                                          */
/*    NC=     500 T= 8.9574E-05 DT= 1.7917E-07 ERR= -0.0% DM/M= 0.0000E+00                                               */
/*    ELAPSED TIME=          0.36 s  REMAINING TIME=        160.90 s                                                     */
/*    NC=    1000 T= 1.7918E-04 DT= 1.7924E-07 ERR= -0.0% DM/M= 0.0000E+00                                               */
/*    ELAPSED TIME=          0.64 s  REMAINING TIME=        143.08 s                                                     */
/*    NC=    1500 T= 2.6881E-04 DT= 1.7929E-07 ERR= -0.0% DM/M= 0.0000E+00                                               */
/*    ...                                                                                                                */
/*    ...                                                                                                                */
/*    NC=  214500 T= 3.9788E-02 DT= 1.8675E-07 ERR= -0.0% DM/M= 0.0000E+00                                               */
/*    ELAPSED TIME=        115.84 s  REMAINING TIME=          0.62 s                                                     */
/*    NC=  215000 T= 3.9882E-02 DT= 1.8679E-07 ERR= -0.0% DM/M= 0.0000E+00                                               */
/*    ELAPSED TIME=        116.07 s  REMAINING TIME=          0.34 s                                                     */
/*    NC=  215500 T= 3.9975E-02 DT= 1.8675E-07 ERR= -0.0% DM/M= 0.0000E+00                                               */
/*    ELAPSED TIME=        116.31 s  REMAINING TIME=          0.07 s                                                     */
/*        RESTART FILES: gmsh_tensile_LAW36_BIQUAD_0001_[0001-0001].rst WRITTEN                                          */
/*        ANIMATION FILE: gmsh_tensile_LAW36_BIQUADA041 WRITTEN                                                          */
/*                                                                                                                       */
/*                              ** CPU USER TIME **                                                                      */
/*                                                                                                                       */
/*                           ** SUMMARY **                                                                               */
/*                                                                                                                       */
/*    #PROC  ELEMENT   KINCOND   INTEG     IO        T0        ASM       RESOL                                           */
/*      1   .9236E+02 .5781E+01 .2609E+01 .3078E+01 .2859E+01 .2344E+01 .1160E+03                                        */
/*                                                                                                                       */
/*                      ** CUMULATIVE CPU TIME SUMMARY **                                                                */
/*                                                                                                                       */
/*    CONTACT SORTING.............: .0000E+00     0.00 %                                                                 */
/*    CONTACT FORCES..............: .6250E-01     0.05 %                                                                 */
/*    ELEMENT FORCES..............: .9236E+02    79.61 %                                                                 */
/*    KINEMATIC COND..............: .5781E+01     4.98 %                                                                 */
/*    INTEGRATION.................: .2609E+01     2.25 %                                                                 */
/*    ASSEMBLING..................: .2344E+01     2.02 %                                                                 */
/*    OTHERS (including I/O)......: .1286E+02    11.08 %                                                                 */
/*    TOTAL.......................: .1160E+03   100.00 %                                                                 */
/*                                                                                                                       */
/*                      ** MEMORY USAGE STATISTICS **                                                                    */
/*                                                                                                                       */
/*    TOTAL MEMORY USED .........................:       49 MB                                                           */
/*    MAXIMUM MEMORY PER PROCESSOR...............:       49 MB                                                           */
/*    MINIMUM MEMORY PER PROCESSOR...............:       49 MB                                                           */
/*    AVERAGE MEMORY PER PROCESSOR...............:       49 MB                                                           */
/*                                                                                                                       */
/*                      ** DISK USAGE STATISTICS **                                                                      */
/*                                                                                                                       */
/*    TOTAL DISK SPACE USED .....................:          6 MB                                                         */
/*    ANIMATION/H3D/TH/OUTP SIZE ................:          5 MB                                                         */
/*    RESTART FILE SIZE .........................:          0 MB                                                         */
/*                                                                                                                       */
/*    ELAPSED TIME     =        116.38 s                                                                                 */
/*                             0:01:56                                                                                   */
/*                                                                                                                       */
/*        NORMAL TERMINATION                                                                                             */
/*        TOTAL NUMBER OF CYCLES  :  215635                                                                              */
/*                                                                                                                       */
/*                                                                                                                       */
/* ======================================================================================================================*/
/*  RESULTS.CSV                                                                                                          */
/* ======================================================================================================================*/
/*                                                                                                                       */
/*   T01 TO CSV CONVERTER                                                                                                */
/*                                                                                                                       */
/*   FILE    = D:\rad\gmsh_tensile_LAW36_BIQUADT01                                                                       */
/*   OUTPUT FILE    = D:\rad\gmsh_tensile_LAW36_BIQUADT01.csv                                                            */
/*    ** CONVERSION COMPLETED                                                                                            */
/*                                                                                                                       */
/* ======================================================================================================================*/
/*  GMSH_TENSILE_LAW36_BIQUADT01 CSVs                                                                                    */
/* ======================================================================================================================*/
/*                                                                                                                       */
/*   d:/rad                                                                                                              */
/*      gmsh_tensile_LAW36_BIQUADT01.csv                                                                                 */
/*      create more easily programmed columh name                                                                        */
/*      gmsh_tensile_LAW36_BIQUADT01_fix.csv                                                                             */
/*                                                                                                                       */
/*                                                                                                                       */
/* ======================================================================================================================*/
/*  GMSH_TENSILE_LAW36_BIQUADT01_FIX.CSV COLUMN NAMES)                                                                   */
/* ======================================================================================================================*/
/*                                                                                                                       */
/*  Middle Observation(2000 ) of table = workx.mydata (whats in the csv)                                                 */
/*                                                                                                                       */
/*  Variable                        Typ    Value                                                                         */
/*                                                                                                                       */
/*  TIME                             N8      0.01999017                                                                  */
/*  INTERNALENERGY                   N8        374710.5                                                                  */
/*  KINETICENERGY                    N8         14.8279                                                                  */
/*  X_MOMENTUM                       N8      0.02961273                                                                  */
/*  Y_MOMENTUM                       N8    -0.000025012                                                                  */
/*  Z_MOMENTUM                       N8     -1.3611E-16                                                                  */
/*  MASS                             N8    0.0000586695                                                                  */
/*  TIMESTEP                         N8     1.870105E-7                                                                  */
/*  ROTATIONENERGY                   N8    2.098867E-28                                                                  */
/*  EXTERNALWORK                     N8        374726.6                                                                  */
/*  SPRINGENERGY                     N8               0                                                                  */
/*  CONTACTENERGY                    N8               0                                                                  */
/*  HOURGLASSENERGY                  N8               0                                                                  */
/*  ELASTICCONTACTENERGY             N8               0                                                                  */
/*  FRICTIONALCONTACTENERGY          N8               0                                                                  */
/*  DAMPINGCONTACTENERGY             N8               0                                                                  */
/*  PLASTICWORK                      N8        369452.4                                                                  */
/*  ADDEDMASS                        N8    1.355253E-20                                                                  */
/*  PERCENTAGEADDEDMASS              N8    2.309977E-14                                                                  */
/*  INLETMASS                        N8               0                                                                  */
/*  OUTLETMASS                       N8               0                                                                  */
/*  INLETENERGY                      N8               0                                                                  */
/*  OUTLETENERGY                     N8               0                                                                  */
/*                                                                                                                       */
/*  TIME HISTORY STRESS, STRAIN and DISPLACEMENT VARIABLES                                                               */
/*                                                                                                                       */
/*  TH_MEASURING_NODES1000002VAR23   N8        19.99017                                                                  */
/*  TH_MEASURING_NODES1000002VAR24   N8               0                                                                  */
/*  TH_MEASURING_NODES1000002VAR25   N8               0                                                                  */
/*  TH_MEASURING_NODES1000002VAR26   N8            1000                                                                  */
/*  TH_MEASURING_NODES1000002VAR27   N8               0                                                                  */
/*  TH_MEASURING_NODES1000002VAR28   N8               0                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR29     N8        374.7015                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR30     N8     0.002103373                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR31     N8    4.560424E-16                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR32     N8    1.682457E-15                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR33     N8    -3.93861E-14                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR34     N8       -6.497537                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR35     N8               0                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR36     N8               0                                                                  */
/*  THRBODY1FIXED_END_RBODYVAR37     N8               0                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR38    N8       -374.7251                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR39    N8    -0.002078436                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR40    N8    -3.19763E-16                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR41    N8    -1.69894E-15                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR42    N8    -3.85494E-14                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR43    N8       0.6221973                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR44    N8               0                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR45    N8               0                                                                  */
/*  THRBODY2MOVING_END_RBODYVAR46    N8               .                                                                  */
/*                                                                                                                       */
/*************************************************************************************************************************/

/*                                    _ _                 _
  ___  _ __   ___ _ __  _ __ __ _  __| (_) ___  ___ ___  | | ___   __ _
 / _ \| `_ \ / _ \ `_ \| `__/ _` |/ _` | |/ _ \/ __/ __| | |/ _ \ / _` |
| (_) | |_) |  __/ | | | | | (_| | (_| | | (_) \__ \__ \ | | (_) | (_| |
 \___/| .__/ \___|_| |_|_|  \__,_|\__,_|_|\___/|___/___/ |_|\___/ \__, |
      |_|                                                         |___/
*/

1                                          Altair SLC        13:07 Wednesday, June  1, 2022

NOTE: Copyright 2002-2025 World Programming, an Altair Company
NOTE: Altair SLC 2026 (05.26.01.00.000758)
      Licensed to Roger DeAngelis
NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
NOTE: AUTOEXEC source line
1       +  ï»¿ods _all_ close;
           ^

NOTE: AUTOEXEC processing completed

1         options validvarname=v7;
2         options set=PYTHONHOME "D:\py314";
3         proc python;
4         submit;
5         import os
6         import pandas as pd
7         import subprocess
8         from pathlib import Path
9
10        # ============================================================================
11        # CONFIGURATION
12        # ============================================================================
13
14        OPENRADIOSS_PATH = Path("C:/openradioss")
15        STARTER_EXE = OPENRADIOSS_PATH / "exec" / "starter_win64.exe"
16        ENGINE_EXE = OPENRADIOSS_PATH / "exec" / "engine_win64.exe"
17        TH_TO_CSV_EXE = OPENRADIOSS_PATH / "exec" / "th_to_csv_win64.exe"
18
19        MODEL_DIR = Path("D:/rad")
20        STARTER_FILE = "gmsh_tensile_LAW36_BIQUAD_0000.rad"
21        ENGINE_FILE = "gmsh_tensile_LAW36_BIQUAD_0001.rad"
22
23        # Time history file (output from simulation)
24        TH_FILE = MODEL_DIR / "gmsh_tensile_LAW36_BIQUADT01"
25        # CSV output file
26        CSV_FILE = MODEL_DIR / "results.csv"
27
28        SETVARS_PATH = Path("C:/Program Files (x86)/Intel/oneAPI/setvars.bat")
29        OMP_NUM_THREADS = "1"
30        KMP_STACKSIZE = "400m"
31
32        # ============================================================================
33        # ENVIRONMENT SETUP
34        # ============================================================================
35
36        def setup_environment():
37            env = os.environ.copy()
38            env["OPENRADIOSS_PATH"] = str(OPENRADIOSS_PATH)
39            env["RAD_CFG_PATH"] = str(OPENRADIOSS_PATH / "hm_cfg_files")
40            env["RAD_H3D_PATH"] = str(OPENRADIOSS_PATH / "extlib" / "h3d" / "lib" / "win64")
41            env["OMP_NUM_THREADS"] = OMP_NUM_THREADS
42            env["KMP_STACKSIZE"] = KMP_STACKSIZE
43
44            hm_reader = OPENRADIOSS_PATH / "extlib" / "hm_reader" / "win64"
45            if hm_reader.exists():
46                env["PATH"] = str(hm_reader) + ";" + env.get("PATH", "")
47
48            # Add tools directory to PATH for th_to_csv
49            tools_dir = OPENRADIOSS_PATH / "tools"
50            if tools_dir.exists():
51                env["PATH"] = str(tools_dir) + ";" + env.get("PATH", "")
52
53            return env
54
55        def run_cmd(cmd, cwd, env, log_file):
56            """Run command and capture output to log file"""
57            with open(log_file, 'w') as f:
58                result = subprocess.run(cmd, shell=True, cwd=cwd, env=env,
59                                        stdout=f, stderr=subprocess.STDOUT, text=True)
60            return result.returncode
61
62        def convert_th_to_csv(th_file, csv_file, env):
63            """Convert time history binary file to CSV format"""
64            if not th_file.exists():
65                print(f"Warning: Time history file not found: {th_file}")
66                return False
67
68            if not TH_TO_CSV_EXE.exists():
69                print(f"Warning: th_to_csv_win64.exe not found at {TH_TO_CSV_EXE}")
70                return False
71
72            print(f"Converting time history: {th_file.name} -> {csv_file.name}")
73
74            # Run th_to_csv and redirect output to CSV file
75            cmd = f'"{TH_TO_CSV_EXE}" "{th_file}"'
76
77            try:
78                with open(csv_file, 'w') as f:
79                    result = subprocess.run(cmd, shell=True, cwd=str(MODEL_DIR), env=env,
80                                            stdout=f, stderr=subprocess.PIPE, text=True)
81
82                if result.returncode == 0 and csv_file.exists() and csv_file.stat().st_size > 0:
83                    # Count lines to verify data
84                    with open(csv_file, 'r') as f:
85                        line_count = sum(1 for _ in f)
86                    print(f"  [OK] Created {csv_file.name} ({line_count} lines, {csv_file.stat().st_size / 1024:.1f} KB)")
87
88                    # Show first few lines as preview
89                    with open(csv_file, 'r') as f:
90                        header = f.readline().strip()
91                        first_data = f.readline().strip() if line_count > 1 else ""
92                    print(f"  Header: {header[:100]}...")
93                    return True
94                else:
95                    print(f"  [FAILED] Conversion failed (exit code {result.returncode})")
96                    if result.stderr:
97                        print(f"    Error: {result.stderr[:200]}")
98                    return False
99            except Exception as e:
100               print(f"  [ERROR] {e}")
101               return False
102
103       # ============================================================================
104       # MAIN EXECUTION
105       # ============================================================================
106
107       def main():
108           print("=" * 60)
109           print("OpenRadioss Tensile Test Simulation")
110           print("=" * 60)
111
112           # Quick verification
113           if not STARTER_EXE.exists() or not ENGINE_EXE.exists():
114               print("ERROR: OpenRadioss executables not found")
115               return 1
116
117           env = setup_environment()
118           setvars = f'call "{SETVARS_PATH}" intel64 vs2022 > nul 2>&1 && ' if SETVARS_PATH.exists() else ""
119
120           # Run Starter
121           print("\n[1/3] Running Starter...")
122           starter_input = MODEL_DIR / STARTER_FILE
123           rc = run_cmd(f'{setvars}"{STARTER_EXE}" -i "{starter_input}"',
124                         str(MODEL_DIR), env, MODEL_DIR / "starter.log")
125           if rc != 0:
126               print(f"Starter failed (exit {rc}). Check starter.log")
127               return rc
128
129           # Run Engine
130           print("[2/3] Running Engine...")
131           engine_input = MODEL_DIR / ENGINE_FILE
132           rc = run_cmd(f'{setvars}"{ENGINE_EXE}" -i "{engine_input}"',
133                         str(MODEL_DIR), env, MODEL_DIR / "engine.log")
134           if rc != 0:
135               print(f"Engine failed (exit {rc}). Check engine.log")
136               return rc
137
138           # List animation files created
139           anim_files = list(MODEL_DIR.glob("*.A0*")) + list(MODEL_DIR.glob("*.h3d"))
140           if anim_files:
141               print(f"\nAnimation files created ({len(anim_files)}):")
142               for f in sorted(anim_files):
143                   size_kb = f.stat().st_size / 1024
144                   print(f"  {f.name} ({size_kb:.1f} KB)")
145           else:
146               print("\nWarning: No animation files found")
147
148           # Convert time history to CSV
149           print("\n[3/3] Converting time history to CSV...")
150           convert_th_to_csv(TH_FILE, CSV_FILE, env)
151
152           # Summary
153           print("\n" + "=" * 60)
154           print("SIMULATION COMPLETE")
155           print("=" * 60)
156           print(f"Results saved to: {MODEL_DIR}")
157           print(f"  - Time history CSV: {CSV_FILE.name}")
158           print(f"  - Starter log: starter.log")
159           print(f"  - Engine log: engine.log")
160           if anim_files:
161               print(f"  - Animation files: {len(anim_files)} files")
162
163           # Verify CSV was created
164           if CSV_FILE.exists():
165               print(f"\nCSV file size: {CSV_FILE.stat().st_size / 1024:.1f} KB")
166               print("You can now open results.csv in Excel or Python for analysis.")
167
168           print("\nDone.")
169           return 0
170
171       if __name__ == "__main__":
172           exit(main());
173       endsubmit;

NOTE: Submitting statements to Python:


174       run;
NOTE: Procedure python step took :
      real time : 2:31.663
      cpu time  : 0:00.000


ERROR: Error printed on page 1

NOTE: Submitted statements took :
      real time : 2:31.780
      cpu time  : 0:00.062
/*  _                      _          _     _     _                    _        _     _
| || |     ___ _____   __ | |_ ___   | |__ (_)___| |_ ___  _ __ _   _ | |_ __ _| |__ | | ___
| || |_   / __/ __\ \ / / | __/ _ \  | `_ \| / __| __/ _ \| `__| | | || __/ _` | `_ \| |/ _ \
|__   _| | (__\__ \\ V /  | || (_) | | | | | \__ \ || (_) | |  | |_| || || (_| | |_) | |  __/
   |_|    \___|___/ \_/    \__\___/  |_| |_|_|___/\__\___/|_|   \__, | \__\__,_|_.__/|_|\___|
                                                                |___/
*/

/*--- PROCESS BELOW
   1  Make csv columns more descrptive
   2  Create sas input program
   3  Add stress strain and variables needed for displacement
---*/

/*--- MAKE COLUMN NAMES MORE CLEAR ---*/

options validvarname=upcase ls=255 ps=255;
data _null_;
  infile "d:/rad/gmsh_tensile_LAW36_BIQUADT01.csv" lrecl=4096;
  file "d:/rad/gmsh_tensile_LAW36_BIQUADT01_fix.csv" lrecl=4096;
  input;
  _infile_=compress(_infile_,'" ');
  if _n_=1 then putlog _infile_;
  put _infile_;
run;

/*--- NEW HEADING ROW

    TIME
    INTERNALENERGY
    KINETICENERGY
    X_MOMENTUM
    Y_MOMENTUM
    Z_MOMENTUM
    MASS
    TIMESTEP
    ROTATIONENERGY
    EXTERNALWORK
    SPRINGENERGY
    CONTACTENERGY
    HOURGLASSENERGY
    ELASTICCONTACTENERGY
    FRICTIONALCONTACTENERGY
    DAMPINGCONTACTENERGY
    PLASTICWORK
    ADDEDMASS
    PERCENTAGEADDEDMASS
    INLETMASS
    OUTLETMASS
    INLETENERGY
    OUTLETENERGY
    TH_MEASURING_NODES1000002VAR23  was TH_Measuring_Nodes                  1000002                              var 23
    TH_MEASURING_NODES1000002VAR24
    TH_MEASURING_NODES1000002VAR25
    TH_MEASURING_NODES1000002VAR26
    TH_MEASURING_NODES1000002VAR27
    TH_MEASURING_NODES1000002VAR28
    THRBODY1FIXED_END_RBODYVAR29    was TH RBODY                            1 Fixed_End_RBODY                    var 29
    THRBODY1FIXED_END_RBODYVAR30
    THRBODY1FIXED_END_RBODYVAR31
    THRBODY1FIXED_END_RBODYVAR32
    THRBODY1FIXED_END_RBODYVAR33
    THRBODY1FIXED_END_RBODYVAR34
    THRBODY1FIXED_END_RBODYVAR35
    THRBODY1FIXED_END_RBODYVAR36
    THRBODY1FIXED_END_RBODYVAR37
    THRBODY2MOVING_END_RBODYVAR38
    THRBODY2MOVING_END_RBODYVAR39
    THRBODY2MOVING_END_RBODYVAR40
    THRBODY2MOVING_END_RBODYVAR41
    THRBODY2MOVING_END_RBODYVAR42
    THRBODY2MOVING_END_RBODYVAR43
    THRBODY2MOVING_END_RBODYVAR44
    THRBODY2MOVING_END_RBODYVAR45
    THRBODY2MOVING_END_RBODYVAR46
---*/


/*--- ONLY USE THIS TO CREATE THE CODE BELOW

PROC IMPORT DATAFILE="d:/rad/gmsh_tensile_LAW36_BIQUADT01_fix.csv"
    OUT=workx.mydata
    DBMS=CSV
    REPLACE;
    GETNAMES=YES;
    GUESSINGROWS=4000;
RUN;
---*/

/*--- CONVERT CSV TO SLC TABLE ---*/

data workx.mydata;
  infile 'd:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv'
      delimiter=','
      MISSOVER DSD
      firstobs=2
      LRECL=32760;
  input
    TIME
    INTERNALENERGY
    KINETICENERGY
    X_MOMENTUM
    Y_MOMENTUM
    Z_MOMENTUM
    MASS
    TIMESTEP
    ROTATIONENERGY
    EXTERNALWORK
    SPRINGENERGY
    CONTACTENERGY
    HOURGLASSENERGY
    ELASTICCONTACTENERGY
    FRICTIONALCONTACTENERGY
    DAMPINGCONTACTENERGY
    PLASTICWORK
    ADDEDMASS
    PERCENTAGEADDEDMASS
    INLETMASS
    OUTLETMASS
    INLETENERGY
    OUTLETENERGY
    TH_MEASURING_NODES1000002VAR23
    TH_MEASURING_NODES1000002VAR24
    TH_MEASURING_NODES1000002VAR25
    TH_MEASURING_NODES1000002VAR26
    TH_MEASURING_NODES1000002VAR27
    TH_MEASURING_NODES1000002VAR28
    THRBODY1FIXED_END_RBODYVAR29
    THRBODY1FIXED_END_RBODYVAR30
    THRBODY1FIXED_END_RBODYVAR31
    THRBODY1FIXED_END_RBODYVAR32
    THRBODY1FIXED_END_RBODYVAR33
    THRBODY1FIXED_END_RBODYVAR34
    THRBODY1FIXED_END_RBODYVAR35
    THRBODY1FIXED_END_RBODYVAR36
    THRBODY1FIXED_END_RBODYVAR37
    THRBODY2MOVING_END_RBODYVAR38
    THRBODY2MOVING_END_RBODYVAR39
    THRBODY2MOVING_END_RBODYVAR40
    THRBODY2MOVING_END_RBODYVAR41
    THRBODY2MOVING_END_RBODYVAR42
    THRBODY2MOVING_END_RBODYVAR43
    THRBODY2MOVING_END_RBODYVAR44
    THRBODY2MOVING_END_RBODYVAR45
    THRBODY2MOVING_END_RBODYVAR46 best32.
;
run;

/*--- ADD STRESS STRAIN AND VARIABLES NEEDED FOR DISPLACEMENT ---*/

options validvarname=upcase ls=255 ps=255;
data workx.tensile_analysis(drop=prev_vx prev_time dt dx );

    retain title "Open Radios Tensile Analysis";

    /* ============================================================================
       OPENRADIOSS TENSILE TEST TIME HISTORY VARIABLES
       Labels for global variables and time history outputs
       ============================================================================ */

    /* Global variables (from /TH/GLOB 0) */
    label
     TIME                             = "Simulation time (ms)"
     STRESS_MPA                       = "Engineering Stress (MPa)"
     PERCENT_STRAIN                   = "Engineering Strain (%)"
     DISPLACEMENT                     = "Displacement"
     WIDTH                            = "Width"
     THICKNESS                        = "Thickness"
     INITIAL_LENGTH                   = "Initial Length"
     ORIGINAL_AREA                    = "Original Area"
     FORCE_N                          = "Force N"
     STRESS_MPA                       = "Engineering Stress (MPa)"
     PERCENT_STRAIN                   = "Engineering Strain (%)"
     STRAIN                           = "Engineering Strain (mm/mm) - dimensionless"
     IMPULSE                          = "Moving End - Resultant force X (N) PRIMARY PULLING FORCE"
     FORCE                            = "Derivative of Impulse Rigid Body/TH RBODY (N)"
     INTERNALENERGY                   = "Total internal energy (N·mm)"
     KINETICENERGY                    = "Total kinetic energy (N·mm)"
     ENERGYBALANCE                    = "(Internalenergy + Kineticenergy) / ExternalWork"
     PLASTICFRACTION                  = "(Plastic Work / Internal Energy)"
     X_MOMENTUM                       = "Linear momentum in X-direction (N·s)"
     Y_MOMENTUM                       = "Linear momentum in Y-direction (N·s)"
     Z_MOMENTUM                       = "Linear momentum in Z-direction (N·s)"
     MASS                             = "Total mass of the structure (kg)"
     TIMESTEP                         = "Current time step (ms)"
     ROTATIONENERGY                   = "Rotational kinetic energy (N·mm)"
     EXTERNALWORK                     = "External work done on the model (N·mm)"
     SPRINGENERGY                     = "Energy stored in spring elements (N·mm)"
     CONTACTENERGY                    = "Total energy dissipated in contacts (N·mm)"
     HOURGLASSENERGY                  = "Hourglass energy (stabilization energy) (N·mm)"
     ELASTICCONTACTENERGY             = "Recoverable energy in contact springs (N·mm)"
     FRICTIONALCONTACTENERGY          = "Energy dissipated by contact friction (N·mm)"
     DAMPINGCONTACTENERGY             = "Energy dissipated by contact damping (N·mm)"
     PLASTICWORK                      = "Plastic dissipation (permanent deformation) (N·mm)"
     ADDEDMASS                        = "Mass added via mass scaling (kg)"
     PERCENTAGEADDEDMASS              = "Added mass as percentage of total mass (%)"
     INLETMASS                        = "Mass entering system through inlets (kg)"
     OUTLETMASS                       = "Mass exiting system through outlets (kg)"
     INLETENERGY                      = "Energy entering system through inlets (N·mm)"
     OUTLETENERGY                     = "Energy exiting system through outlets (N·mm)"

     /* TH_MEASURING_NODES1000002 (Node 1000002 - Moving end measurement point)
        These typically include: D (displacement), V (velocity), A (acceleration),
        TP (total displacement), PC (distance between two nodes), DT (distance change),
        DEF (deformation), and related components */
     TH_MEASURING_NODES1000002VAR23   = "Node 1000002 - Displacement X (mm)"
     TH_MEASURING_NODES1000002VAR24   = "Node 1000002 - Displacement Y (mm)"
     TH_MEASURING_NODES1000002VAR25   = "Node 1000002 - Displacement Z (mm)"
     TH_MEASURING_NODES1000002VAR26   = "Node 1000002 - Total displacement magnitude (mm)"
     TH_MEASURING_NODES1000002VAR27   = "Node 1000002 - Velocity X (mm/ms)"
     TH_MEASURING_NODES1000002VAR28   = "Node 1000002 - Velocity Y (mm/ms)"

     /* THRBODY1FIXED_END_RBODY (Fixed end rigid body - Left side)
        VAR29: Fx, VAR30: Fy, VAR31: Fz, VAR32: Mx, VAR33: My, VAR34: Mz,
        VAR35: Vx, VAR36: Vy, VAR37: Vz */
     THRBODY1FIXED_END_RBODYVAR29     = "Fixed End - Resultant force X (N)"
     THRBODY1FIXED_END_RBODYVAR30     = "Fixed End - Resultant force Y (N)"
     THRBODY1FIXED_END_RBODYVAR31     = "Fixed End - Resultant force Z (N)"
     THRBODY1FIXED_END_RBODYVAR32     = "Fixed End - Resultant moment X (N·mm)"
     THRBODY1FIXED_END_RBODYVAR33     = "Fixed End - Resultant moment Y (N·mm)"
     THRBODY1FIXED_END_RBODYVAR34     = "Fixed End - Resultant moment Z (N·mm)"
     THRBODY1FIXED_END_RBODYVAR35     = "Fixed End - Velocity X (mm/ms)"
     THRBODY1FIXED_END_RBODYVAR36     = "Fixed End - Velocity Y (mm/ms)"
     THRBODY1FIXED_END_RBODYVAR37     = "Fixed End - Velocity Z (mm/ms)"

     /* THRBODY2MOVING_END_RBODY (Moving end rigid body - Right side)
        VAR38: Fx, VAR39: Fy, VAR40: Fz, VAR41: Mx, VAR42: My, VAR43: Mz,
        VAR44: Vx, VAR45: Vy, VAR46: Vz */
     THRBODY2MOVING_END_RBODYVAR38    = "Moving End - Resultant force X (N) *** PRIMARY PULLING FORCE ***"
     THRBODY2MOVING_END_RBODYVAR39    = "Moving End - Resultant force Y (N)"
     THRBODY2MOVING_END_RBODYVAR40    = "Moving End - Resultant force Z (N)"
     THRBODY2MOVING_END_RBODYVAR41    = "Moving End - Resultant moment X (N·mm)"
     THRBODY2MOVING_END_RBODYVAR42    = "Moving End - Resultant moment Y (N·mm)"
     THRBODY2MOVING_END_RBODYVAR43    = "Moving End - Resultant moment Z (N·mm)"
     THRBODY2MOVING_END_RBODYVAR44    = "Moving End - Velocity X (mm/ms)"
     THRBODY2MOVING_END_RBODYVAR45    = "Moving End - Velocity Y (mm/ms)"
     THRBODY2MOVING_END_RBODYVAR46    = "Moving End - Velocity Z (mm/ms)"
    ;
    set workx.mydata;
        /* Geometry */
    initial_length = 200;
    width = 12;
    thickness = 2.5;
    original_area = width * thickness;  /* 30 mm^2 */

    /* DO NOT multiply by 1000 - force is already in Newtons */
    force_N = THRBODY1Fixed_End_RBODYvar29;  /* Already in N */

    /* Stress (MPa = N/mm²) */
    stress_mpa = force_N / original_area;

    /* Displacement integration */
    retain dx 0;
    prev_vx   = lag(TH_Measuring_Nodes1000002var23);
    prev_time = lag(time);
    dt        = time - prev_time;

    if _n_ = 1 then dx = 0;
    else if dt > 0 then do;
        dx = dx + (TH_Measuring_Nodes1000002var23 + prev_vx) / 2 * dt;
    end;

    displacement=dx;

    /* Strain */
    strain = dx / initial_length;
    percent_strain = strain * 100;

    /* Filter */
    if stress_mpa > 0;

    /* Debug output */
    if _n_ <= 5 then do;
        put "Row " _n_ ": Force = " force_N " N, Stress = " stress_mpa " MPa, Strain = " percent_strain "%";
    end;

    /* force rigid body / th rigid body */

    impulse = THRBODY2MOVING_END_RBODYVAR38;
    force = -dif(impulse) / dif(time);

    energybalance=(Internalenergy + Kineticenergy) / ExternalWork;
    plasticfraction = (PlasticWork / InternalEnergy);

run;

proc means data=workx.tensile_analysis n mean min max;
    var  stress_mpa strain percent_strain displacement;
    title "Summary Statistics";
run;

/**************************************************************************************************************************/
/*                                                                                                                        */
/* ==================                                                                                                     */
/* SUMMARY STATISTICS                                                                                                     */
/* ==================                                                                                                     */
/*                                                                                                                        */
/* Variable          Label                                            N            Mean         Minimum         Maximum   */
/* --------------------------------------------------------------------------------------------------------------------   */
/* STRESS_MPA        Engineering Stress (MPa)                      3998       9.5866146    2.322515E-19      13.1957733   */
/* STRAIN            Engineering Strain (mm/mm) - dimensionless    3998       0.0013335    1.0061946E-9       0.0039980   */
/* PERCENT_STRAIN    Engineering Strain (%)                        3998       0.1333510    1.0061946E-7       0.3998026   */
/* DISPLACEMENT      Displacement                                  3998       0.2667019    2.0123891E-7       0.7996053   */
/* --------------------------------------------------------------------------------------------------------------------   */                                                                                                        */
/*                                                                                                                        */
/* ================================                                                                                       */
/* Validation (DP600 plastic region                                                                                       */
/* ================================                                                                                       */
/*                                                                                                                        */
/* Metric               Observed       Expected       Status                                                              */
/* Maximum Stress      13.20 MPa*     ~13-15 MPa*     Correct                                                             */
/* Mean Stress          9.59 MPa#      ~9-11 MPa*     Correct                                                             */
/* Maximum Strain       0.40 %          0.40%         Correct                                                             */
/* Displacement        0.7996053       ~6-.85         Correct                                                             */
/*                                                                                                                        */
/* *At 0.4% strain, DP600 is just past yield, so stress should be in the elastic-plastic transition                       */
/*                                                                                                                        */
/* ======================                                                                                                 */
/* WORKX.TENSILE_ANALYSIS                                                                                                 */
/* ======================                                                                                                 */
/*                                                                                                                        */
/* Middle Observation(1999 ) of table = workx.tensile_analysis - Total Obs 3998                                           */
/*                                                                                                                        */
/*  -- CHARACTER --                                                                                                       */
/* Variable                        Typ    Value                      Label                                                */
/*                                                                                                                        */
/* TITLE                            C28   Open Radios Tensile AnalysiTITLE                                                */
/* TOTOBS                           C16   3,998                      TOTOBS                                               */
/*                                                                                                                        */
/*                                                                                                                        */
/*  -- NUMERIC --                                                                                                         */
/* TIME                             N8      0.02000009    Simulation time (ms)                                            */
/* STRESS_MPA                       N8    12.495816667    Engineering Stress (MPa)                                        */
/* PERCENT_STRAIN                   N8    0.1000009004    Engineering Strain (%)                                          */
/* DISPLACEMENT                     N8    0.2000018008    Displacement                                                    */
/* WIDTH                            N8              12    Width                                                           */
/* THICKNESS                        N8             2.5    Thickness                                                       */
/* INITIAL_LENGTH                   N8             200    Initial Length                                                  */
/* ORIGINAL_AREA                    N8              30    Original Area                                                   */
/* FORCE_N                          N8        374.8745    Force N                                                         */
/* STRAIN                           N8     0.001000009    Engineering Strain (mm/mm) - dimensionless                      */
/* IMPULSE                          N8       -374.8982    Moving End - Resultant force X (N) PRIMARY PULLING FORCE        */
/* FORCE                            N8    17449.596774    Derivative of Impulse Rigid Body/TH RBODY (N)                   */
/* INTERNALENERGY                   N8        374883.5    Total internal energy (N·mm)                                    */
/* KINETICENERGY                    N8        14.82358    Total kinetic energy (N·mm)                                     */
/* X_MOMENTUM                       N8      0.02961205    Linear momentum in X-direction (N·s)                            */
/* Y_MOMENTUM                       N8    -0.000016574    Linear momentum in Y-direction (N·s)                            */
/* Z_MOMENTUM                       N8    -1.41635E-16    Linear momentum in Z-direction (N·s)                            */
/* MASS                             N8    0.0000586695    Total mass of the structure (kg)                                */
/* TIMESTEP                         N8     1.870104E-7    Current time step (ms)                                          */
/* ROTATIONENERGY                   N8    2.165672E-28    Rotational kinetic energy (N·mm)                                */
/* EXTERNALWORK                     N8        374899.6    External work done on the model (N·mm)                          */
/* SPRINGENERGY                     N8               0    Energy stored in spring elements (N·mm)                         */
/* CONTACTENERGY                    N8               0    Total energy dissipated in contacts (N·mm)                      */
/* HOURGLASSENERGY                  N8               0    Hourglass energy (stabilization energy) (N·mm)                  */
/* ELASTICCONTACTENERGY             N8               0    Recoverable energy in contact springs (N·mm)                    */
/* FRICTIONALCONTACTENERGY          N8               0    Energy dissipated by contact friction (N·mm)                    */
/* DAMPINGCONTACTENERGY             N8               0    Energy dissipated by contact damping (N·mm)                     */
/* PLASTICWORK                      N8        369628.7    Plastic dissipation (permanent deformation) (N·mm)              */
/* ADDEDMASS                        N8    1.355253E-20    Mass added via mass scaling (kg)                                */
/* PERCENTAGEADDEDMASS              N8    2.309977E-14    Added mass as percentage of total mass (%)                      */
/* INLETMASS                        N8               0    Mass entering system through inlets (kg)                        */
/* OUTLETMASS                       N8               0    Mass exiting system through outlets (kg)                        */
/* INLETENERGY                      N8               0    Energy entering system through inlets (N·mm)                    */
/* OUTLETENERGY                     N8               0    Energy exiting system through outlets (N·mm)                    */
/*                                                                                                                        */
/* TH_MEASURING_NODES1000002 (Node 1000002 - Moving end measurement point)                                                */
/* These typically include: D (displacement), V (velocity), A (acceleration),                                             */
/* TP (total displacement), PC (distance between two nodes), DT (distance change),                                        */
/* DEF (deformation), and related components                                                                              */
/*                                                                                                                        */
/* TH_MEASURING_NODES1000002VAR23   N8        20.00008    Node 1000002 - Displacement X (mm)                              */
/* TH_MEASURING_NODES1000002VAR24   N8               0    Node 1000002 - Displacement Y (mm)                              */
/* TH_MEASURING_NODES1000002VAR25   N8               0    Node 1000002 - Displacement Z (mm)                              */
/* TH_MEASURING_NODES1000002VAR26   N8            1000    Node 1000002 - Total displacement magnitude (mm)                */
/* TH_MEASURING_NODES1000002VAR27   N8               0    Node 1000002 - Velocity X (mm/ms)                               */
/* TH_MEASURING_NODES1000002VAR28   N8               0    Node 1000002 - Velocity Y (mm/ms)                               */
/*                                                                                                                        */
/* THRBODY1FIXED_END_RBODY (Fixed end rigid body - Left side)                                                             */
/* VAR29: Fx, VAR30: Fy, VAR31: Fz, VAR32: Mx, VAR33: My, VAR34: Mz,                                                      */
/* VAR35: Vx, VAR36: Vy, VAR37: Vz                                                                                        */
/*                                                                                                                        */
/* THRBODY1FIXED_END_RBODYVAR29     N8        374.8745    Fixed End - Resultant force X (N)                               */
/* THRBODY1FIXED_END_RBODYVAR30     N8     0.002095145    Fixed End - Resultant force Y (N)                               */
/* THRBODY1FIXED_END_RBODYVAR31     N8     4.48317E-16    Fixed End - Resultant force Z (N)                               */
/* THRBODY1FIXED_END_RBODYVAR32     N8    1.640384E-15    Fixed End - Resultant moment X (N·mm)                           */
/* THRBODY1FIXED_END_RBODYVAR33     N8    -3.94127E-14    Fixed End - Resultant moment Y (N·mm)                           */
/* THRBODY1FIXED_END_RBODYVAR34     N8       -6.501127    Fixed End - Resultant moment Z (N·mm)                           */
/* THRBODY1FIXED_END_RBODYVAR35     N8               0    Fixed End - Velocity X (mm/ms)                                  */
/* THRBODY1FIXED_END_RBODYVAR36     N8               0    Fixed End - Velocity Y (mm/ms)                                  */
/* THRBODY1FIXED_END_RBODYVAR37     N8               0    Fixed End - Velocity Z (mm/ms)                                  */
/*                                                                                                                        */
/* THRBODY2MOVING_END_RBODY (Moving end rigid body - Right side)                                                          */
/* VAR38: Fx, VAR39: Fy, VAR40: Fz, VAR41: Mx, VAR42: My, VAR43: Mz,                                                      */
/* VAR44: Vx, VAR45: Vy, VAR46: Vz                                                                                        */
/*                                                                                                                        */
/* THRBODY2MOVING_END_RBODYVAR38    N8       -374.8982    Moving End - Resultant force X (N) *** PRIMARY PULLING FORCE ****/
/* THRBODY2MOVING_END_RBODYVAR39    N8    -0.002078652    Moving End - Resultant force Y (N)                              */
/* THRBODY2MOVING_END_RBODYVAR40    N8     -3.0626E-16    Moving End - Resultant force Z (N)                              */
/* THRBODY2MOVING_END_RBODYVAR41    N8    -1.76498E-15    Moving End - Resultant moment X (N·mm)                          */
/* THRBODY2MOVING_END_RBODYVAR42    N8    -3.84263E-14    Moving End - Resultant moment Y (N·mm)                          */
/* THRBODY2MOVING_END_RBODYVAR43    N8       0.6220268    Moving End - Resultant moment Z (N·mm)                          */
/* THRBODY2MOVING_END_RBODYVAR44    N8               0    Moving End - Velocity X (mm/ms)                                 */
/* THRBODY2MOVING_END_RBODYVAR45    N8               0    Moving End - Velocity Y (mm/ms)                                 */
/* THRBODY2MOVING_END_RBODYVAR46    N8               .    Moving End - Velocity Z (mm/ms)                                 */
/*                                                                                                                        */
/**************************************************************************************************************************/


/*
| | ___   __ _
| |/ _ \ / _` |
| | (_) | (_| |
|_|\___/ \__, |
         |___/
*/

1                                          Altair SLC        16:43 Wednesday, June  1, 2022

NOTE: Copyright 2002-2025 World Programming, an Altair Company
NOTE: Altair SLC 2026 (05.26.01.00.000758)
      Licensed to Roger DeAngelis
NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
NOTE: AUTOEXEC source line
1       +  ï»¿ods _all_ close;
           ^
ERROR: Expected a statement keyword : found "?"
NOTE: Library workx assigned as follows:
      Engine:        SAS7BDAT
      Physical Name: d:\wpswrkx

NOTE: Library wpdx assigned as follows:
      Engine:        WPD
      Physical Name: d:\wpswrkx

NOTE: Library slchelp assigned as follows:
      Engine:        WPD
      Physical Name: C:\Progra~1\Altair\SLC\2026\sashelp


LOG:  16:43:03
NOTE: 1 record was written to file PRINT

NOTE: The data step took :
      real time : 0.026
      cpu time  : 0.000


NOTE: Format num2mis output
NOTE: Format $chr2mis output
NOTE: Procedure format step took :
      real time : 0.003
      cpu time  : 0.000


NOTE: AUTOEXEC processing completed

1
2         /*--- PROCESS BELOW
3            1  Make csv columns more descrptive
4            2  Create sas input program
5            3  Add stress strain and variables needed for displacement
6         ---*/
7
8         /*--- MAKE COLUMN NAMES MOE USABLE ---*/
9
10        options validvarname=upcase ls=255 ps=255;
11        data _null_;
12          infile "d:/rad/gmsh_tensile_LAW36_BIQUADT01.csv" lrecl=4096;
13          file "d:/rad/gmsh_tensile_LAW36_BIQUADT01_fix.csv" lrecl=4096;
14          input;
15          _infile_=compress(_infile_,'" ');
16          if _n_=1 then putlog _infile_;
17          put _infile_;
18        run;

NOTE: The infile 'd:\rad\gmsh_tensile_LAW36_BIQUADT01.csv' is:
      Filename='d:\rad\gmsh_tensile_LAW36_BIQUADT01.csv',
      Owner Name=SLC\suzie,
      File size (bytes)=2483695,
      Create Time=16:42:03 Jun 01 2022,
      Last Accessed=16:42:03 Jun 01 2022,
      Last Modified=16:42:03 Jun 01 2022,
      Lrecl=4096, Recfm=V

NOTE: The file 'd:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv' is:
      Filename='d:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv',
      Owner Name=SLC\suzie,
      File size (bytes)=0,
      Create Time=16:43:02 Jun 01 2022,
      Last Accessed=16:43:02 Jun 01 2022,
      Last Modified=16:43:02 Jun 01 2022,
      Lrecl=4096, Recfm=V

time,INTERNALENERGY,KINETICENERGY,X-MOMENTUM,Y-MOMENTUM,Z-MOMENTUM,MASS,TIMESTEP,ROTATIONENERGY,EXTERNALWORK,SPRINGENERGY,CONTACTENERGY,HOURGLASSENERGY,ELASTICCONTACTENERGY,FRICTIONALCONTACTENERGY,DAMPINGCONTACTENERGY,PLASTICWORK,ADDEDMASS,PERCENTAGEADDED
MASS,INLETMASS,OUTLETMASS,INLETENERGY,OUTLETENERGY,TH_Measuring_Nodes1000002var23,TH_Measuring_Nodes1000002var24,TH_Measuring_Nodes1000002var25,TH_Measuring_Nodes1000002var26,TH_Measuring_Nodes1000002var27,TH_Measuring_Nodes1000002var28,THRBODY1Fixed_End_
RBODYvar29,THRBODY1Fixed_End_RBODYvar30,THRBODY1Fixed_End_RBODYvar31,THRBODY1Fixed_End_RBODYvar32,THRBODY1Fixed_End_RBODYvar33,THRBODY1Fixed_End_RBODYvar34,THRBODY1Fixed_End_RBODYvar35,THRBODY1Fixed_End_RBODYvar36,THRBODY1Fixed_End_RBODYvar37,THRBODY2Movi
ng_End_RBODYvar38,THRBODY2Moving_End_RBODYvar39,THRBODY2Moving_End_RBODYvar40,THRBODY2Moving_End_RBODYvar41,THRBODY2Moving_End_RBODYvar42,THRBODY2Moving_End_RBODYvar43,THRBODY2Moving_End_RBODYvar44,THRBODY2Moving_End_RBODYvar45,THRBODY2Moving_End_RBODYvar
46
NOTE: 4001 records were read from file 'd:\rad\gmsh_tensile_LAW36_BIQUADT01.csv'
      The minimum record length was 610
      The maximum record length was 2642
NOTE: 4001 records were written to file 'd:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv'
      The minimum record length was 610
      The maximum record length was 1022
NOTE: The data step took :
      real time : 0.037
      cpu time  : 0.031


19
20        /*--- NEW HEADING ROW
21
22            TIME
23            INTERNALENERGY
24            KINETICENERGY
25            X_MOMENTUM
26            Y_MOMENTUM
27            Z_MOMENTUM
28            MASS
29            TIMESTEP
30            ROTATIONENERGY
31            EXTERNALWORK
32            SPRINGENERGY
33            CONTACTENERGY
34            HOURGLASSENERGY
35            ELASTICCONTACTENERGY
36            FRICTIONALCONTACTENERGY
37            DAMPINGCONTACTENERGY
38            PLASTICWORK
39            ADDEDMASS
40            PERCENTAGEADDEDMASS
41            INLETMASS
42            OUTLETMASS
43            INLETENERGY
44            OUTLETENERGY
45            TH_MEASURING_NODES1000002VAR23  was TH_Measuring_Nodes                  1000002                              var 23
46            TH_MEASURING_NODES1000002VAR24
47            TH_MEASURING_NODES1000002VAR25
48            TH_MEASURING_NODES1000002VAR26
49            TH_MEASURING_NODES1000002VAR27
50            TH_MEASURING_NODES1000002VAR28
51            THRBODY1FIXED_END_RBODYVAR29    was TH RBODY                            1 Fixed_End_RBODY                    var 29
52            THRBODY1FIXED_END_RBODYVAR30
53            THRBODY1FIXED_END_RBODYVAR31
54            THRBODY1FIXED_END_RBODYVAR32
55            THRBODY1FIXED_END_RBODYVAR33
56            THRBODY1FIXED_END_RBODYVAR34
57            THRBODY1FIXED_END_RBODYVAR35
58            THRBODY1FIXED_END_RBODYVAR36
59            THRBODY1FIXED_END_RBODYVAR37
60            THRBODY2MOVING_END_RBODYVAR38
61            THRBODY2MOVING_END_RBODYVAR39
62            THRBODY2MOVING_END_RBODYVAR40
63            THRBODY2MOVING_END_RBODYVAR41
64            THRBODY2MOVING_END_RBODYVAR42
65            THRBODY2MOVING_END_RBODYVAR43
66            THRBODY2MOVING_END_RBODYVAR44
67            THRBODY2MOVING_END_RBODYVAR45
68            THRBODY2MOVING_END_RBODYVAR46
69        ---*/
70
71
72        /*--- ONLY USE THIS TO CREATE THE CODE BELOW
73
74        PROC IMPORT DATAFILE="d:/rad/gmsh_tensile_LAW36_BIQUADT01_fix.csv"
75            OUT=workx.mydata
76            DBMS=CSV
77            REPLACE;
78            GETNAMES=YES;
79            GUESSINGROWS=4000;
80        RUN;
81        ---*/
82
83        /*--- CONVERT CSV TO SLC TABLE ---*/
84
85        data workx.mydata;
86          infile 'd:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv'
87              delimiter=','
88              MISSOVER DSD
89              firstobs=2
90              LRECL=32760;
91          input
92            TIME
93            INTERNALENERGY
94            KINETICENERGY
95            X_MOMENTUM
96            Y_MOMENTUM
97            Z_MOMENTUM
98            MASS
99            TIMESTEP
100           ROTATIONENERGY
101           EXTERNALWORK
102           SPRINGENERGY
103           CONTACTENERGY
104           HOURGLASSENERGY
105           ELASTICCONTACTENERGY
106           FRICTIONALCONTACTENERGY
107           DAMPINGCONTACTENERGY
108           PLASTICWORK
109           ADDEDMASS
110           PERCENTAGEADDEDMASS
111           INLETMASS
112           OUTLETMASS
113           INLETENERGY
114           OUTLETENERGY
115           TH_MEASURING_NODES1000002VAR23
116           TH_MEASURING_NODES1000002VAR24
117           TH_MEASURING_NODES1000002VAR25
118           TH_MEASURING_NODES1000002VAR26
119           TH_MEASURING_NODES1000002VAR27
120           TH_MEASURING_NODES1000002VAR28
121           THRBODY1FIXED_END_RBODYVAR29
122           THRBODY1FIXED_END_RBODYVAR30
123           THRBODY1FIXED_END_RBODYVAR31
124           THRBODY1FIXED_END_RBODYVAR32
125           THRBODY1FIXED_END_RBODYVAR33
126           THRBODY1FIXED_END_RBODYVAR34
127           THRBODY1FIXED_END_RBODYVAR35
128           THRBODY1FIXED_END_RBODYVAR36
129           THRBODY1FIXED_END_RBODYVAR37
130           THRBODY2MOVING_END_RBODYVAR38
131           THRBODY2MOVING_END_RBODYVAR39
132           THRBODY2MOVING_END_RBODYVAR40
133           THRBODY2MOVING_END_RBODYVAR41
134           THRBODY2MOVING_END_RBODYVAR42
135           THRBODY2MOVING_END_RBODYVAR43
136           THRBODY2MOVING_END_RBODYVAR44
137           THRBODY2MOVING_END_RBODYVAR45
138           THRBODY2MOVING_END_RBODYVAR46 best32.
139       ;
140       run;

NOTE: The infile 'd:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv' is:
      Filename='d:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv',
      Owner Name=SLC\suzie,
      File size (bytes)=2482075,
      Create Time=16:43:02 Jun 01 2022,
      Last Accessed=16:43:02 Jun 01 2022,
      Last Modified=16:43:02 Jun 01 2022,
      Lrecl=32760, Recfm=V

NOTE: 4000 records were read from file 'd:\rad\gmsh_tensile_LAW36_BIQUADT01_fix.csv'
      The minimum record length was 610
      The maximum record length was 621
NOTE: Data set "WORKX.mydata" has 4000 observation(s) and 47 variable(s)
NOTE: The data step took :
      real time : 0.079
      cpu time  : 0.062


141
142       /*--- ADD STRESS STRAIN AND VARIABLES NEEDED FOR DISPLACEMENT ---*/
143
144       options validvarname=upcase ls=255 ps=255;
145       data workx.tensile_analysis(drop=prev_vx prev_time dt dx );
146
147           retain title "Open Radios Tensile Analysis";
148
149           /* ============================================================================
150              OPENRADIOSS TENSILE TEST TIME HISTORY VARIABLES
151              Labels for global variables and time history outputs
152              ============================================================================ */
153
154           /* Global variables (from /TH/GLOB 0) */
155           label
156            TIME                             = "Simulation time (ms)"
157            STRESS_MPA                       = "Engineering Stress (MPa)"
158            PERCENT_STRAIN                   = "Engineering Strain (%)"
159            DISPLACEMENT                     = "Displacement"

2                                                                                                                         Altair SLC

160            WIDTH                            = "Width"
161            THICKNESS                        = "Thickness"
162            INITIAL_LENGTH                   = "Initial Length"
163            ORIGINAL_AREA                    = "Original Area"
164            FORCE_N                          = "Force N"
165            STRESS_MPA                       = "Engineering Stress (MPa)"
166            PERCENT_STRAIN                   = "Engineering Strain (%)"
167            STRAIN                           = "Engineering Strain (mm/mm) - dimensionless"
168            IMPULSE                          = "Moving End - Resultant force X (N) PRIMARY PULLING FORCE"
169            FORCE                            = "Derivative of Impulse Rigid Body/TH RBODY (N)"
170            INTERNALENERGY                   = "Total internal energy (NÂ·mm)"
171            KINETICENERGY                    = "Total kinetic energy (NÂ·mm)"
172            ENERGYBALANCE                    = "(Internalenergy + Kineticenergy) / ExternalWork"
173            PLASTICFRACTION                  = "(Plastic Work / Internal Energy)"
174            X_MOMENTUM                       = "Linear momentum in X-direction (NÂ·s)"
175            Y_MOMENTUM                       = "Linear momentum in Y-direction (NÂ·s)"
176            Z_MOMENTUM                       = "Linear momentum in Z-direction (NÂ·s)"
177            MASS                             = "Total mass of the structure (kg)"
178            TIMESTEP                         = "Current time step (ms)"
179            ROTATIONENERGY                   = "Rotational kinetic energy (NÂ·mm)"
180            EXTERNALWORK                     = "External work done on the model (NÂ·mm)"
181            SPRINGENERGY                     = "Energy stored in spring elements (NÂ·mm)"
182            CONTACTENERGY                    = "Total energy dissipated in contacts (NÂ·mm)"
183            HOURGLASSENERGY                  = "Hourglass energy (stabilization energy) (NÂ·mm)"
184            ELASTICCONTACTENERGY             = "Recoverable energy in contact springs (NÂ·mm)"
185            FRICTIONALCONTACTENERGY          = "Energy dissipated by contact friction (NÂ·mm)"
186            DAMPINGCONTACTENERGY             = "Energy dissipated by contact damping (NÂ·mm)"
187            PLASTICWORK                      = "Plastic dissipation (permanent deformation) (NÂ·mm)"
188            ADDEDMASS                        = "Mass added via mass scaling (kg)"
189            PERCENTAGEADDEDMASS              = "Added mass as percentage of total mass (%)"
190            INLETMASS                        = "Mass entering system through inlets (kg)"
191            OUTLETMASS                       = "Mass exiting system through outlets (kg)"
192            INLETENERGY                      = "Energy entering system through inlets (NÂ·mm)"
193            OUTLETENERGY                     = "Energy exiting system through outlets (NÂ·mm)"
194
195            /* TH_MEASURING_NODES1000002 (Node 1000002 - Moving end measurement point)
196               These typically include: D (displacement), V (velocity), A (acceleration),
197               TP (total displacement), PC (distance between two nodes), DT (distance change),
198               DEF (deformation), and related components */
199            TH_MEASURING_NODES1000002VAR23   = "Node 1000002 - Displacement X (mm)"
200            TH_MEASURING_NODES1000002VAR24   = "Node 1000002 - Displacement Y (mm)"
201            TH_MEASURING_NODES1000002VAR25   = "Node 1000002 - Displacement Z (mm)"
202            TH_MEASURING_NODES1000002VAR26   = "Node 1000002 - Total displacement magnitude (mm)"
203            TH_MEASURING_NODES1000002VAR27   = "Node 1000002 - Velocity X (mm/ms)"
204            TH_MEASURING_NODES1000002VAR28   = "Node 1000002 - Velocity Y (mm/ms)"
205
206            /* THRBODY1FIXED_END_RBODY (Fixed end rigid body - Left side)
207               VAR29: Fx, VAR30: Fy, VAR31: Fz, VAR32: Mx, VAR33: My, VAR34: Mz,
208               VAR35: Vx, VAR36: Vy, VAR37: Vz */
209            THRBODY1FIXED_END_RBODYVAR29     = "Fixed End - Resultant force X (N)"
210            THRBODY1FIXED_END_RBODYVAR30     = "Fixed End - Resultant force Y (N)"
211            THRBODY1FIXED_END_RBODYVAR31     = "Fixed End - Resultant force Z (N)"
212            THRBODY1FIXED_END_RBODYVAR32     = "Fixed End - Resultant moment X (NÂ·mm)"
213            THRBODY1FIXED_END_RBODYVAR33     = "Fixed End - Resultant moment Y (NÂ·mm)"
214            THRBODY1FIXED_END_RBODYVAR34     = "Fixed End - Resultant moment Z (NÂ·mm)"
215            THRBODY1FIXED_END_RBODYVAR35     = "Fixed End - Velocity X (mm/ms)"
216            THRBODY1FIXED_END_RBODYVAR36     = "Fixed End - Velocity Y (mm/ms)"
217            THRBODY1FIXED_END_RBODYVAR37     = "Fixed End - Velocity Z (mm/ms)"
218
219            /* THRBODY2MOVING_END_RBODY (Moving end rigid body - Right side)
220               VAR38: Fx, VAR39: Fy, VAR40: Fz, VAR41: Mx, VAR42: My, VAR43: Mz,
221               VAR44: Vx, VAR45: Vy, VAR46: Vz */
222            THRBODY2MOVING_END_RBODYVAR38    = "Moving End - Resultant force X (N) *** PRIMARY PULLING FORCE ***"
223            THRBODY2MOVING_END_RBODYVAR39    = "Moving End - Resultant force Y (N)"
224            THRBODY2MOVING_END_RBODYVAR40    = "Moving End - Resultant force Z (N)"
225            THRBODY2MOVING_END_RBODYVAR41    = "Moving End - Resultant moment X (NÂ·mm)"
226            THRBODY2MOVING_END_RBODYVAR42    = "Moving End - Resultant moment Y (NÂ·mm)"
227            THRBODY2MOVING_END_RBODYVAR43    = "Moving End - Resultant moment Z (NÂ·mm)"
228            THRBODY2MOVING_END_RBODYVAR44    = "Moving End - Velocity X (mm/ms)"
229            THRBODY2MOVING_END_RBODYVAR45    = "Moving End - Velocity Y (mm/ms)"
230            THRBODY2MOVING_END_RBODYVAR46    = "Moving End - Velocity Z (mm/ms)"
231           ;
232           set workx.mydata;
233               /* Geometry */
234           initial_length = 200;
235           width = 12;
236           thickness = 2.5;
237           original_area = width * thickness;  /* 30 mm^2 */
238
239           /* DO NOT multiply by 1000 - force is already in Newtons */
240           force_N = THRBODY1Fixed_End_RBODYvar29;  /* Already in N */
241
242           /* Stress (MPa = N/mmÂ²) */
243           stress_mpa = force_N / original_area;
244
245           /* Displacement integration */
246           retain dx 0;
247           prev_vx   = lag(TH_Measuring_Nodes1000002var23);
248           prev_time = lag(time);
249           dt        = time - prev_time;
250
251           if _n_ = 1 then dx = 0;
252           else if dt > 0 then do;
253               dx = dx + (TH_Measuring_Nodes1000002var23 + prev_vx) / 2 * dt;
254           end;
255
256           displacement=dx;
257
258           /* Strain */
259           strain = dx / initial_length;
260           percent_strain = strain * 100;
261
262           /* Filter */
263           if stress_mpa > 0;
264
265           /* Debug output */
266           if _n_ <= 5 then do;
267               put "Row " _n_ ": Force = " force_N " N, Stress = " stress_mpa " MPa, Strain = " percent_strain "%";
268           end;
269
270           /* force rigid body / th rigid body */
271
272           impulse = THRBODY2MOVING_END_RBODYVAR38;
273           force = -dif(impulse) / dif(time);
274
275           energybalance=(Internalenergy + Kineticenergy) / ExternalWork;
276           plasticfraction = (PlasticWork / InternalEnergy);
277
278       run;

Row 3 : Force = 6.967544E-18  N, Stress = 2.322515E-19  MPa, Strain = 1.0061946E-7 %
Row 4 : Force = 0.0000264271  N, Stress = 8.8090467E-7  MPa, Strain = 2.2639656E-7 %
Row 5 : Force = 0.02533407  N, Stress = 0.000844469  MPa, Strain = 4.0251356E-7 %
NOTE: Missing values resulted from performing arithmetic upon missing values.
      Each place is given by: (Number of times) at (Line):(Column)
      1 at 249:22   1 at 273:13   1 at 273:14   1 at 273:27   1 at 273:29
NOTE: 4000 observations were read from "WORKX.mydata"
NOTE: Data set "WORKX.tensile_analysis" has 3998 observation(s) and 61 variable(s)
NOTE: The data step took :
      real time : 0.027
      cpu time  : 0.015


279
280       proc means data=workx.tensile_analysis n mean min max;
281           var  stress_mpa strain percent_strain displacement;
282           title "Summary Statistics";
283       run;
NOTE: 3998 observations were read from "WORKX.tensile_analysis"
NOTE: Procedure means step took :
      real time : 0.040
      cpu time  : 0.015


ERROR: Error printed on page 1

NOTE: Submitted statements took :
      real time : 0.267
      cpu time  : 0.171




/*___               _ _     _       _   _                     _       _
| ___|  __   ____ _| (_) __| | __ _| |_(_) ___  _ __    _ __ | | ___ | |_
|___ \  \ \ / / _` | | |/ _` |/ _` | __| |/ _ \| `_ \  | `_ \| |/ _ \| __|
 ___) |  \ V / (_| | | | (_| | (_| | |_| | (_) | | | | | |_) | | (_) | |_
|____/    \_/ \__,_|_|_|\__,_|\__,_|\__|_|\___/|_| |_| | .__/|_|\___/ \__|
                                                       |_|
*/

/*--- DETERMINE THE NECKING TIME AND FORCE ---*/

proc sql;
  select
    min(time) as necking_start
   ,force
  from
     workx.tensile_analysis
  where
    (select
      max(force) as max_force
    from
      workx.tensile_analysis) = force
;quit;

/*---
For validation an exact reproduction of the published plot,Fixed_End_RBODY_FX_X, at
https://openradioss.atlassian.net/wiki/spaces/OPENRADIOSS/pages/24444938/Tensile+Test+Example+Tutorial+Using+Gmsh is created.
---*/

%utlfkil(d:/rad/Fixed_End_RBODY_FX_X_FORCE.png);

ods listing close;
ods graphics on / reset=all imagename="Fixed_End_RBODY_FX_X" outputfmt=png;
ods printer file="d:/rad/Fixed_End_RBODY_FX_X_FORCE.png" dpi=100;
/* Step 10: Create displacement vs force plot (raw data) */
proc sgplot data=workx.tensile_analysis;
    series x=time y=force ;
    xaxis label="Time s" grid;
    yaxis label="Rigid Body/TH RBODY (N)" grid;
    title "Fixed End RBODY FX X FORCE";
    inset "Necking Start Time: 0.01446 s"
          "Rigid Body/TH RBODY N-Force: 20,585 N" /
          position=topright border;
    refline 0.01446 / axis=x ;
run;
ods printer close;
ods graphics off;
ods listing;

options ls=64 ps=44;
proc plot data=workx.tensile_analysis;
  plot force*time='*'/box haxis=0 to .04 by .005 href=0.01446;
run;quit;
options ls=255 ps=255;

/**************************************************************************************************************************/
/*                                                                                                                        */
/* Stress-Strain Curve for DP600 Steel Tensile Test                                                                       */
/* Initial Length = 200 mm, Gauge Width = 12 mm                                                                           */
/*                                                                                                                        */
/*               Derivative                                                                                               */
/*               of Impulse                                                                                               */
/*                    Rigid                                                                                               */
/*     necking_     Body/TH                                                                                               */
/*        start   RBODY (N)                                                                                               */
/* ------------------------                                                                                               */
/*      0.01446    20585.27                                                                                               */
/*                                                                                                                        */
/*            Plot of FORCE*TIME.  Symbol used is '*'.                                                                    */
/*                                                                                                                        */
/*        0.000 0.005 0.010 0.015 0.020 0.025 0.030 0.035 0.040                                                           */
/*       ---+-----+-----+-----+-----+-----+-----+-----+-----+---                                                          */
/* FORCE |                 0.01446 Necking Starts              | FORCE (N)                                                */
/*       | Peak Force is     V                                 |                                                          */
/* 25000 + where Necking     |                                 + 25000                                                    */
/*       | Starts            |                                 |                                                          */
/*       |                   | -> Necking Starts 0.01446       |                                                          */
/*       |                   | -> Necking Force (N) 20,585     |                                                          */
/*       |                 ****                                |                                                          */
/* 20000 +          ******** |***                              + 20000                                                    */
/*       |        ***        |  ***                            |                                                          */
/*       |      ***          |    ***                          |                                                          */
/*       |     **            |      **                         |                                                          */
/*       |    **             |      **                         |                                                          */
/* 15000 +   **              |       *                         + 15000                                                    */
/*       |   *               |       *                         |                                                          */
/*       |   *               |       *                         |                                                          */
/*       |  **               |       *                         |                                                          */
/*       |  *                |       *                         |                                                          */
/* 10000 +  *                |       *                         + 10000                                                    */
/*       |  *                |       **                        |                                                          */
/*       |  *                |       **                        |                                                          */
/*       |  *                |       **                        |                                                          */
/*       |  *                |       *                         |                                                          */
/*  5000 +  *                |        **                       +  5000                                                    */
/*       |  *                |        ******                   |                                                          */
/*       |  *                |        *************            |                                                          */
/*       |  *                |        ***********************  |                                                          */
/*       |  *                |        ***********************  |                                                          */
/*     0 +                   |        ***********************  +     0                                                    */
/*       |                   |        ***********************  |                                                          */
/*       |                   |        ***********************  |                                                          */
/*       |                   |        *************            |                                                          */
/*       |                   |        ***                      |                                                          */
/* -5000 +                   |                                 + -5000                                                    */
/*       |                0,01446 Necking Starts               |                                                          */
/*       ---+-----+-----+-----+-----+-----+-----+-----+-----+---                                                          */
/*        0.000 0.005 0.010 0.015 0.020 0.025 0.030 0.035 0.040                                                           */
/*                                                                                                                        */
/*                        Simulation time (ms)                                                                            */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*
| | ___   __ _
| |/ _ \ / _` |
| | (_) | (_| |
|_|\___/ \__, |
         |___/
*/

1                                          Altair SLC          12:19 Saturday, May 16, 2026

NOTE: Copyright 2002-2025 World Programming, an Altair Company
NOTE: Altair SLC 2026 (05.26.01.00.000758)
      Licensed to Roger DeAngelis
NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
NOTE: AUTOEXEC source line
1       +  ï»¿ods _all_ close;
           ^
ERROR: Expected a statement keyword : found "?"
NOTE: Library workx assigned as follows:
      Engine:        SAS7BDAT
      Physical Name: d:\wpswrkx

NOTE: Library wpdx assigned as follows:
      Engine:        WPD
      Physical Name: d:\wpswrkx

NOTE: Library slchelp assigned as follows:
      Engine:        WPD
      Physical Name: C:\Progra~1\Altair\SLC\2026\sashelp


LOG:  12:19:11
NOTE: 1 record was written to file PRINT

NOTE: The data step took :
      real time : 0.015
      cpu time  : 0.015


NOTE: Format num2mis output
NOTE: Format $chr2mis output
NOTE: Procedure format step took :
      real time : 0.015
      cpu time  : 0.000


NOTE: AUTOEXEC processing completed

1
2         /*--- DETERMINE THE NECKING TIME AND FORCE ---*/
3
4         proc sql;
5           select
6             min(time) as necking_start
7            ,force
8           from
9              workx.tensile_analysis
10          where
11            (select
12              max(force) as max_force
13            from
14              workx.tensile_analysis) = force
15        ;quit;
NOTE: Summary results are required to be remerged with the original data
NOTE: Procedure sql step took :
      real time : 0.131
      cpu time  : 0.000


16
17        /*---
18        For validation an exact reproduction of the published plot,Fixed_End_RBODY_FX_X, at
19        https://openradioss.atlassian.net/wiki/spaces/OPENRADIOSS/pages/24444938/Tensile+Test+Example+Tutorial+Using+Gmsh is created.
20        ---*/
21
22        %utlfkil(d:/rad/Fixed_End_RBODY_FX_X_FORCE.png);
The file d:/rad/Fixed_End_RBODY_FX_X_FORCE.png does not exist
23
24        ods listing close;
25        ods graphics on / reset=all imagename="Fixed_End_RBODY_FX_X" outputfmt=png;
26        ods printer file="d:/rad/Fixed_End_RBODY_FX_X_FORCE.png" dpi=100;
WARNING: ODS PRINTER is currently EXPERIMENTAL and is subject to change
27        /* Step 10: Create displacement vs force plot (raw data) */
28        proc sgplot data=workx.tensile_analysis;
29            series x=time y=force ;
30            xaxis label="Time s" grid;
31            yaxis label="Rigid Body/TH RBODY (N)" grid;
32            title "Fixed End RBODY FX X FORCE";
33            inset "Necking Start Time: 0.01446 s"
34                  "Rigid Body/TH RBODY N-Force: 20,585 N" /
35                  position=topright border;
36            refline 0.01446 / axis=x ;
37        run;
NOTE: Procedure sgplot step took :
      real time : 0.315
      cpu time  : 0.562


NOTE: Writing file d:\rad\Fixed_End_RBODY_FX_X_FORCE.png
38        ods printer close;
39        ods graphics off;
40        ods listing;
41
42        options ls=64 ps=44;
43        proc plot data=workx.tensile_analysis;
44          plot force*time='*'/box haxis=0 to .04 by .005 href=
44      ! 0.01446;
45        run;quit;
NOTE: 1 observation(s) outside the axis range for the Plot of
      FORCE*TIME. Symbol used is '*'. request
NOTE: Procedure plot step took :
      real time : 0.015
      cpu time  : 0.031


46        options ls=255 ps=255;
47
ERROR: Error printed on page 1

NOTE: Submitted statements took :
      real time : 0.940
      cpu time  : 0.875


/*__         _        _
 / /_    ___| |_ __ _| |_   ___ _   _ _ __ ___  _ __ ___   __ _ _ __ _   _
| `_ \  / __| __/ _` | __| / __| | | | `_ ` _ \| `_ ` _ \ / _` | `__| | | |
| (_) | \__ \ || (_| | |_  \__ \ |_| | | | | | | | | | | | (_| | |  | |_| |
 \___/  |___/\__\__,_|\__| |___/\__,_|_| |_| |_|_| |_| |_|\__,_|_|   \__, |
                                                                     |___/
     12. Statistical Summary Report:
*/

options ls=255;
/* Generate summary statistics */
proc means data=workx.tensile_analysis n mean std min max;
run;

The MEANS Procedure

Variable                          Label                                                                  N            Mean         Std Dev         Minimum         Maximum
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
TIME                              Simulation time (ms)                                               3998       0.0200051       0.0115427     0.000020062       0.0399901
STRESS_MPA                        Engineering Stress (MPa)                                           3998       9.5866146       4.4629992    2.322515E-19      13.1957733
PERCENT_STRAIN                    Engineering Strain (%)                                             3998       0.1333510       0.1192370    1.0061946E-7       0.3998026
DISPLACEMENT                      Displacement                                                       3998       0.2667019       0.2384740    2.0123891E-7       0.7996053
WIDTH                             Width                                                              3998      12.0000000               0      12.0000000      12.0000000
THICKNESS                         Thickness                                                          3998       2.5000000               0       2.5000000       2.5000000
INITIAL_LENGTH                    Initial Length                                                     3998     200.0000000               0     200.0000000     200.0000000
ORIGINAL_AREA                     Original Area                                                      3998      30.0000000               0      30.0000000      30.0000000
FORCE_N                           Force N                                                            3998     287.5984382     133.8899769    6.967544E-18     395.8732000
STRAIN                            Engineering Strain (mm/mm) - dimensionless                         3998       0.0013335       0.0011924    1.0061946E-9       0.0039980
IMPULSE                           Moving End - Resultant force X (N) PRIMARY PULLING FORCE           3998    -287.6220326     133.8901622    -395.9007000      -0.0293606
FORCE                             Derivative of Impulse Rigid Body/TH RBODY (N)                      3997         9902.87         9487.32        -3949.49        20585.27
INTERNALENERGY                    Total internal energy (NAmm)                                       3998       287568.19       133855.79      13.4613800       395794.50
KINETICENERGY                     Total kinetic energy (NAmm)                                        3998      34.9358147      32.1266947       6.2850530     278.8622000
X_MOMENTUM                        Linear momentum in X-direction (NAs)                               3998       0.0295826       0.0186777      -0.0476979       0.1086515
Y_MOMENTUM                        Linear momentum in Y-direction (NAs)                               3998     0.000028659       0.0089523      -0.0409704       0.0516043
Z_MOMENTUM                        Linear momentum in Z-direction (NAs)                               3998    2.091393E-18    6.652578E-17    -2.21397E-16    2.284042E-16
MASS                              Total mass of the structure (kg)                                   3998     0.000058670               0     0.000058670     0.000058670
TIMESTEP                          Current time step (ms)                                             3998    1.8552966E-7     2.229927E-9     1.787926E-7     1.870557E-7
ROTATIONENERGY                    Rotational kinetic energy (NAmm)                                   3998    1.170778E-28    8.412621E-29     4.33835E-32    3.314421E-28
EXTERNALWORK                      External work done on the model (NAmm)                             3998       287624.15       133890.77      32.2420300       395903.40
SPRINGENERGY                      Energy stored in spring elements (NAmm)                            3998               0               0               0               0
CONTACTENERGY                     Total energy dissipated in contacts (NAmm)                         3998               0               0               0               0
HOURGLASSENERGY                   Hourglass energy (stabilization energy) (NAmm)                     3998               0               0               0               0
ELASTICCONTACTENERGY              Recoverable energy in contact springs (NAmm)                       3998               0               0               0               0
FRICTIONALCONTACTENERGY           Energy dissipated by contact friction (NAmm)                       3998               0               0               0               0
DAMPINGCONTACTENERGY              Energy dissipated by contact damping (NAmm)                        3998               0               0               0               0
PLASTICWORK                       Plastic dissipation (permanent deformation) (NAmm)                 3998       284218.95       134963.29               0       394441.70
ADDEDMASS                         Mass added via mass scaling (kg)                                   3998    1.355253E-20               0    1.355253E-20    1.355253E-20
PERCENTAGEADDEDMASS               Added mass as percentage of total mass (%)                         3998    2.309977E-14               0    2.309977E-14    2.309977E-14
INLETMASS                         Mass entering system through inlets (kg)                           3998               0               0               0               0
OUTLETMASS                        Mass exiting system through outlets (kg)                           3998               0               0               0               0
INLETENERGY                       Energy entering system through inlets (NAmm)                       3998               0               0               0               0
OUTLETENERGY                      Energy exiting system through outlets (NAmm)                       3998               0               0               0               0
TH MEASURING_NODES1000002VAR23    Node 1000002 - Displacement X (mm)                                 3998      20.0050938      11.5426752       0.0200619      39.9901300
TH_MEASURING_NODES1000002VAR24    Node 1000002 - Displacement Y (mm)                                 3998               0               0               0               0
TH_MEASURING_NODES1000002VAR25    Node 1000002 - Displacement Z (mm)                                 3998               0               0               0               0
TH_MEASURING_NODES1000002VAR26    Node 1000002 - Total displacement magnitude (mm)                   3998         1000.00               0         1000.00         1000.00
TH_MEASURING_NODES1000002VAR27    Node 1000002 - Velocity X (mm/ms)                                  3998               0               0               0               0
TH_MEASURING_NODES1000002VAR28    Node 1000002 - Velocity Y (mm/ms)                                  3998               0               0               0               0
THRBODY1FIXED_END_RBODYVAR29      Fixed End - Resultant force X (N)                                  3998     287.5984382     133.8899769    6.967544E-18     395.8732000
THRBODY1FIXED_END_RBODYVAR30      Fixed End - Resultant force Y (N)                                  3998       0.0043272       0.0080858      -0.0235263       0.0362914
THRBODY1FIXED_END_RBODYVAR31      Fixed End - Resultant force Z (N)                                  3998     2.89162E-16    1.505161E-16    -1.45223E-17    5.794693E-16
THRBODY1FIXED_END_RBODYVAR32      Fixed End - Resultant moment X (NAmm)                              3998    5.853682E-17    1.101644E-15    -2.81936E-15    2.005138E-15
THRBODY1FIXED_END_RBODYVAR33      Fixed End - Resultant moment Y (NAmm)                              3998     -2.8342E-14    1.305008E-14    -4.72174E-14    1.597453E-16
THRBODY1FIXED_END_RBODYVAR34      Fixed End - Resultant moment Z (NAmm)                              3998      -4.6383845       2.0495222      -7.2611170     0.000728079
THRBODY1FIXED_END_RBODYVAR35      Fixed End - Velocity X (mm/ms)                                     3998               0               0               0               0
THRBODY1FIXED_END_RBODYVAR36      Fixed End - Velocity Y (mm/ms)                                     3998               0               0               0               0
THRBODY1FIXED_END_RBODYVAR37      Fixed End - Velocity Z (mm/ms)                                     3998               0               0               0               0
THRBODY2MOVING_END_RBODYVAR38     Moving End - Resultant force X (N) *** PRIMARY PULLING FORCE ***   3998    -287.6220326     133.8901622    -395.9007000      -0.0293606
THRBODY2MOVING_END_RBODYVAR39     Moving End - Resultant force Y (N)                                 3998      -0.0043558       0.0063283      -0.0309968       0.0128824
THRBODY2MOVING_END_RBODYVAR40     Moving End - Resultant force Z (N)                                 3998    -2.91248E-16    1.439227E-16    -5.07088E-16     1.92986E-17
THRBODY2MOVING_END_RBODYVAR41     Moving End - Resultant moment X (NAmm)                             3998    -1.45823E-15    1.094025E-15    -2.86324E-15    9.851281E-17
THRBODY2MOVING_END_RBODYVAR42     Moving End - Resultant moment Y (NAmm)                             3998    -2.90873E-14    1.577033E-14    -4.63019E-14    1.729071E-15
THRBODY2MOVING_END_RBODYVAR43     Moving End - Resultant moment Z (NAmm)                             3998       0.6957725       0.4479677    -0.000664637       1.9484600
THRBODY2MOVING_END_RBODYVAR44     Moving End - Velocity X (mm/ms)                                    3998               0               0               0               0
THRBODY2MOVING_END_RBODYVAR45     Moving End - Velocity Y (mm/ms)                                    3998               0               0               0               0
THRBODY2MOVING_END_RBODYVAR46     Moving End - Velocity Z (mm/ms)                                       0               .               .               .               .
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

/*
| | ___   __ _
| |/ _ \ / _` |
| | (_) | (_| |
|_|\___/ \__, |
         |___/
*/

1                                          Altair SLC          13:13 Saturday, May 16, 2026

NOTE: Copyright 2002-2025 World Programming, an Altair Company
NOTE: Altair SLC 2026 (05.26.01.00.000758)
      Licensed to Roger DeAngelis
NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
NOTE: AUTOEXEC source line
1       +  ï»¿ods _all_ close;
           ^

NOTE: AUTOEXEC processing completed

1          options ls=255;
2         /* Generate summary statistics */
3         proc means data=workx.tensile_analysis n mean std min max;
4         run;
NOTE: 3998 observations were read from "WORKX.tensile_analysis"
NOTE: Procedure means step took :
      real time : 0.111
      cpu time  : 0.093

5
ERROR: Error printed on page 1

NOTE: Submitted statements took :
      real time : 0.190
      cpu time  : 0.156


/*____                                       _       _
|___  |  _ __ ___   __ _ _ __  _   _   _ __ | | ___ | |_ ___
   / /  | `_ ` _ \ / _` | `_ \| | | | | `_ \| |/ _ \| __/ __|
  / /   | | | | | | (_| | | | | |_| | | |_) | | (_) | |_\__ \
 /_/    |_| |_| |_|\__,_|_| |_|\__, | | .__/|_|\___/ \__|___/
                               |___/  |_|
*/

/*--- STRESS-STRAIN CURVE FOR DP600 STEEL TENSILE TEST ---*/

%utlfkil(d:/rad/stress_strain.png);

ods listing close;
ods graphics on / reset=all imagename="stress_strain" outputfmt=png;
ods printer file="d:/rad/stress_strain.png" dpi=100;

proc sgplot data=workx.tensile_analysis(where=(time>0.0002));
    series x=percent_strain y=stress_mpa ;
    xaxis label="Engineering Strain (%)" grid;
    yaxis label="Engineering Stress (MPa)" grid;
    title "Stress-Strain Curve for DP600 Steel Tensile Test";
    title2 "Initial Length = 200 mm, Gauge Width = 12 mm";
run;quit;

ods printer close;
ods graphics off;
ods listing;


/*--- FORCE VS DISPLACEMENT - RAW TENSILE TEST DATA ---*/

%utlfkil(d:/rad/force_displace.png);

ods listing close;
ods graphics on / reset=all imagename="force_displace" outputfmt=png;
ods printer file="d:/rad/force_displace.png" dpi=100;
/* Step 10: Create displacement vs force plot (raw data) */
proc sgplot data=workx.tensile_analysis;
    series x=displacement y=force ;
    xaxis label="Displacement (mm)" grid;
    yaxis label="Force (N)" grid;
    title "Force vs Displacement - Raw Tensile Test Data";
run;
ods printer close;
ods graphics off;
ods listing;


/*---  ENERGY EVOLUTION - EARLY PHASE (FIRST 1 TIME UNIT) ---*/

%utlfkil(d:/rad/energy_evolution.png);

ods listing close;
ods graphics on / reset=all imagename="energy_evolution" outputfmt=png;
ods printer file="d:/rad/energy_evolution.png" dpi=100;
proc sgplot data=workx.tensile_analysis; /* First 1 time unit for clarity */

    title "Energy Evolution - Early Phase (First 1 Time Unit)";
    title2 "Internal vs Kinetic Energy";

    series x=time y=INTERNALENERGY /
           lineattrs=(color=blue thickness=2)
           name="Internal"
           legendlabel="Internal Energy";

    series x=time y=KINETICENERGY /
           lineattrs=(color=red thickness=2)
           y2axis
           name="Kinetic"
           legendlabel="Kinetic Energy";

    series x=time y=PLASTICWORK /
           lineattrs=(color=green thickness=2 pattern=shortdash)
           name="Plastic"
           legendlabel="Plastic Work";

    xaxis label="Time" grid;
    yaxis label="Internal Energy & Plastic Work" grid;
    y2axis label="Kinetic Energy";

    keylegend "Internal" "Kinetic" "Plastic" /
               position=topright across=1;
run;

ods printer close;
ods graphics off;
ods listing;


/*--- Energy Evolution - Full Simulation (0-0.018 Time Units)  ---*/

%utlfkil(d:/ead/energy_evolutionfull.png);

ods listing close;
ods graphics on / reset=all imagename="energy_evolutionfull" outputfmt=png;
ods printer file="d:/rad/energy_evolutionfull.png" dpi=100;
/* Full time scale energy plot */
proc sgplot data=workx.tensile_analysis;
    title "Energy Evolution - Full Simulation (0-0.018 Time Units)";

    series x=time y=INTERNALENERGY /
           lineattrs=(color=blue thickness=2)
           name="Internal"
           legendlabel="Internal Energy";

    series x=time y=PLASTICWORK /
           lineattrs=(color=green thickness=2 pattern=dash)
           name="Plastic"
           legendlabel="Plastic Work";

    xaxis label="Time" grid ;
    yaxis label="Energy" grid;

    keylegend "Internal" "Plastic" / position=topright;
run;quit;

ods printer close;
ods graphics off;
ods listing;


/*--- X-Momentum Evolution Primary Direction Impulse ---*/

%utlfkil(d:/rad/xmomentumevolve.png);

ods listing close;
ods graphics on / reset=all imagename="xmomentumevolve" outputfmt=png;
ods printer file="d:/rad/xmomentumevolve.png" dpi=100;

proc sgplot data=workx.tensile_analysis;
    title "X-Momentum Evolution";
    title2 "Primary Direction Impulse";

    series x=time y=X_MOMENTUM /
           lineattrs=(color=darkblue thickness=2)
           markerattrs=(color=darkblue symbol=circlefilled size=5);

    refline 0.0293349 / axis=y label="Steady State (0.0293349)"
                     lineattrs=(color=green pattern=dash);

    xaxis label="Time" grid;
    yaxis label="X-Momentum" grid;
run;

ods printer close;
ods _all_ close;
ods listing;


/*--- Time Step Evolution Solver Adaptation to Simulation Complexity ---*/

%utlfkil(d:/rad/timestephistory.png);

ods listing close;
ods graphics on / reset=all imagename="timestephistory" outputfmt=png;
ods printer file="d:/rad/timestephistory.png" dpi=100;

/* Time step history with change detection */
proc sgplot data=workx.tensile_analysis;
    title "Time Step Evolution";
    title2 "Solver Adaptation to Simulation Complexity";

    /* Use scatter for better visibility of changes */
    scatter x=time y=TIMESTEP /
            markerattrs=(color=red symbol=circle size=6);

    /* Smooth line through the points */
    pbspline x=time y=TIMESTEP /
             lineattrs=(color=blue thickness=1)
             nomarkers;

    xaxis label="Time" grid;
    yaxis label="Time Step"
          type=log /* Log scale often helpful for time step */
          logbase=10
          logstyle=linear
          grid;

    inset "Initial Time Step: 0.001"
          "Final Time Step: 0.04" /
          position=bottomright border;
run;quit;

ods printer close;
ods graphics off;
ods listing;


/*--- Energy Balance Verification Plot ---*/

%utlfkil(d:/rad/energybalance.png);

ods listing close;
ods graphics on / reset=all imagename="energybalance" outputfmt=png;
ods printer file="d:/rad/energybalance.png" dpi=100;

/* Energy conservation check */
proc sgplot data=workx.tensile_analysis;
    title "Energy Balance Verification";
    title2 "(Internal + Kinetic) / External Work";

     band x=time upper=1.0005 lower=0.9995 /
         fillattrs=(color=lightgreen transparency=0.7)
         legendlabel="±5% Tolerance";

    series x=time y=ENERGYBALANCE /
           lineattrs=(color=black thickness=2);

    refline 1 / axis=y label="Perfect Conservation"
                lineattrs=(color=red pattern=dash thickness=1.5);

    xaxis label="Time" ;
    yaxis label="Energy Balance Ratio"
          values=(0.9995 to 1.0005 by 0.0002)
          ;
run;

ods printer close;
ods graphics off;
ods listing;



/*--- Fraction of Internal Energy from Plastic Deformation ---*/

%utlfkil(d:/rad/plasticwork.png);

ods listing close;
ods graphics on / reset=all imagename="plasticwork" outputfmt=png;
ods printer file="d:/rad/plasticwork.png" dpi=100;


/* Plastic work fraction */
proc sgplot data=workx.tensile_analysis(where=(INTERNALENERGY > 0));
    title "Plastic Work Development";
    title2 "Fraction of Internal Energy from Plastic Deformation";

    series x=time y=PLASTICFRACTION /
           lineattrs=(color=red thickness=2);
    /*
           markers
           markerattrs=(color=brown symbol=diamondfilled size=6) ;
    */

    refline 0.997 / axis=y label="Final Ratio (0.983)"
                    lineattrs=(color=green pattern=dash);

    xaxis label="Time" grid;
    yaxis label="Plastic Work / Internal Energy"
          values=(0 to 1 by 0.1)
          grid;

    inset "Final State: 98.3% Plastic" /
          position=bottomright border textattrs=(size=10);
run;

ods printer close;
ods graphics off;
ods listing;
/*
| | ___   __ _
| |/ _ \ / _` |
| | (_) | (_| |
|_|\___/ \__, |
         |___/
NOT INCLUDED - REPETION OF VALIDATION PLOT LOG
*/

/*___                    _                 _   _               _               _   _       __ _ _
 ( _ )    __ _ _ __ ___ (_)_ __ ___   __ _| |_(_) ___  _ __   | |_ ___  __   _| |_| | __  / _(_) | ___  ___
 / _ \   / _` | `_ ` _ \| | `_ ` _ \ / _` | __| |/ _ \| `_ \  | __/ _ \ \ \ / / __| |/ / | |_| | |/ _ \/ __|
| (_) | | (_| | | | | | | | | | | | | (_| | |_| | (_) | | | | | || (_) | \ V /| |_|   <  |  _| | |  __/\__ \
 \___/   \__,_|_| |_| |_|_|_| |_| |_|\__,_|\__|_|\___/|_| |_|  \__\___/   \_/  \__|_|\_\ |_| |_|_|\___||___/
*/


/*--- CONVERT ANIMATION FILES TO VTK ---*/

options validvarname=v7;
options set=PYTHONHOME "D:\py314";
proc python;
submit;
import subprocess
import os
from pathlib import Path

# Binary-safe conversion
rad_dir = Path("D:/rad")
converter = Path("C:/openradioss/exec/anim_to_vtk_win64.exe")

# Find all ANIM files (no extension, contains pattern)
anim_files = [f for f in rad_dir.iterdir()
              if f.is_file() and "gmsh_tensile_LAW36_BIQUADA" in f.name and f.suffix == ""]

for anim_file in sorted(anim_files):
    output_file = anim_file.with_suffix(".vtk")
    print(f"Converting: {anim_file.name}")

    # Binary-safe: Use subprocess with stdout capture as bytes
    result = subprocess.run(
        [str(converter), str(anim_file)],
        capture_output=True,
        check=False
    )

    # Write binary output directly (no text conversion)
    with open(output_file, 'wb') as f:
        f.write(result.stdout)

    if result.returncode == 0 and output_file.stat().st_size > 0:
        # Verify VTK header
        with open(output_file, 'rb') as f:
            if f.read(5) == b'# vtk':
                print(f"  [OK] Valid VTK file ({output_file.stat().st_size} bytes)")
            else:
                print(f"  [WARNING] Invalid VTK header")
    else:
        print(f"  [FAILED] Return code: {result.returncode}")
endsubmit;
run;

/**************************************************************************************************************************/
/*                                                                                                                        */
/* gmsh_tensile_LAW36_BIQUADA001      113,325 INPUT                                                                       */
/* gmsh_tensile_LAW36_BIQUADA001.vtk  238,354 OUTPUT                                                                      */
/*                                                                                                                        */
/* gmsh_tensile_LAW36_BIQUADA002      113,325                                                                             */
/* gmsh_tensile_LAW36_BIQUADA002.vtk  452,231                                                                             */
/*                                                                                                                        */
/* gmsh_tensile_LAW36_BIQUADA003      113,325                                                                             */
/* gmsh_tensile_LAW36_BIQUADA003.vtk  449,053                                                                             */
/*                                                                                                                        */
/*                                                                                                                        */
/* gmsh_tensile_LAW36_BIQUADA039      113,325                                                                             */
/* gmsh_tensile_LAW36_BIQUADA039.vtk  440,534                                                                             */
/*                                                                                                                        */
/* gmsh_tensile_LAW36_BIQUADA040      113,325                                                                             */
/* gmsh_tensile_LAW36_BIQUADA040.vtk  439,854                                                                             */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*
| | ___   __ _
| |/ _ \ / _` |
| | (_) | (_| |
|_|\___/ \__, |
         |___/
*/

1                                          Altair SLC          10:57 Saturday, May 16, 2026

NOTE: Copyright 2002-2025 World Programming, an Altair Company
NOTE: Altair SLC 2026 (05.26.01.00.000758)
      Licensed to Roger DeAngelis
NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
NOTE: AUTOEXEC source line
1       +  ï»¿ods _all_ close;
           ^
ERROR: Expected a statement keyword : found "?"

NOTE: AUTOEXEC processing completed

1         options validvarname=v7;
2         options set=PYTHONHOME "D:\py314";
3         proc python;
4         submit;
5         import subprocess
6         import os
7         from pathlib import Path
8
9         # Binary-safe conversion
10        rad_dir = Path("D:/rad")
11        converter = Path("C:/openradioss/exec/anim_to_vtk_win64.exe")
12
13        # Find all ANIM files (no extension, contains pattern)
14        anim_files = [f for f in rad_dir.iterdir()
15                      if f.is_file() and "gmsh_tensile_LAW36_BIQUADA" in f.name and f.suffix == ""]
16
17        for anim_file in sorted(anim_files):
18            output_file = anim_file.with_suffix(".vtk")
19            print(f"Converting: {anim_file.name}")
20
21            # Binary-safe: Use subprocess with stdout capture as bytes
22            result = subprocess.run(
23                [str(converter), str(anim_file)],
24                capture_output=True,
25                check=False
26            )
27
28            # Write binary output directly (no text conversion)
29            with open(output_file, 'wb') as f:
30                f.write(result.stdout)
31
32            if result.returncode == 0 and output_file.stat().st_size > 0:
33                # Verify VTK header
34                with open(output_file, 'rb') as f:
35                    if f.read(5) == b'# vtk':
36                        print(f"  [OK] Valid VTK file ({output_file.stat().st_size} bytes)")
37                    else:
38                        print(f"  [WARNING] Invalid VTK header")
39            else:
40                print(f"  [FAILED] Return code: {result.returncode}")
41        endsubmit;

NOTE: Submitting statements to Python:


42        run;
NOTE: Procedure python step took :
      real time : 5.609
      cpu time  : 0.015


ERROR: Error printed on page 1

NOTE: Submitted statements took :
      real time : 5.694
      cpu time  : 0.078

/*___          _   _      _                          _  _
 / _ \  __   _| |_| | __ | |_ ___    _ __ ___  _ __ | || |
| (_) | \ \ / / __| |/ / | __/ _ \  | `_ ` _ \| `_ \| || |_
 \__, |  \ V /| |_|   <  | || (_) | | | | | | | |_) |__   _|
   /_/    \_/  \__|_|\_\  \__\___/  |_| |_| |_| .__/   |_|
                                              |_|
*/

/*--- DISPLAY ANIMATION INTERACTIVELY AND CREATE MP4 FILE FOR PLAYING LATER ---*/

%slc_pvpybegin;
cards4;
#!/usr/bin/env python
"""
ParaView 6.1.0 script - Save animation as PNG sequence
Then convert to MP4 using FFmpeg
"""

from paraview.simple import *
import glob
import os
import subprocess

# ============================================================
# CONFIGURATION
# ============================================================
VTK_FILE_PATTERN = "D:/rad/gmsh_tensile_LAW36_BIQUADA*.vtk"
OUTPUT_DIR = "D:/rad/frames"
OUTPUT_VIDEO = "D:/rad/animation.mp4"
FRAME_RATE = 10

# Image resolution (use EVEN numbers for H.264)
IMAGE_WIDTH = 1920
IMAGE_HEIGHT = 1080

# Axis scaling - make specimen wider
X_SCALE = 1.0
Y_SCALE = 1.0
Z_SCALE = 20.0

# Camera settings (top view)
CAMERA_POSITION = [0, 0, 15]
CAMERA_FOCAL_POINT = [0, 0, 0]
CAMERA_VIEW_UP = [0, 1, 0]
USE_PARALLEL_PROJECTION = True
PARALLEL_SCALE = 15.0

# Create output directory
os.makedirs(OUTPUT_DIR, exist_ok=True)

print("=" * 60)
print("ParaView 6.1.0 - Creating Animation via PNG Sequence")
print("=" * 60)
print(f"Output video: {OUTPUT_VIDEO}")
print(f"Frame rate: {FRAME_RATE} fps")
print(f"Z-axis scale: {Z_SCALE}x (wider specimen)")

# Find and load files
vtk_files = sorted(glob.glob(VTK_FILE_PATTERN))

if not vtk_files:
    print(f"ERROR: No VTK files found matching {VTK_FILE_PATTERN}")
    exit(1)

print(f"Found {len(vtk_files)} VTK files")

# Load data
reader = LegacyVTKReader(FileNames=vtk_files)

# Apply scaling to make specimen wider
transform = Transform(Input=reader)
transform.Transform = 'Transform'
transform.Transform.Scale = [X_SCALE, Y_SCALE, Z_SCALE]

# Create render view
renderView = CreateView('RenderView')
renderView.ViewSize = [IMAGE_WIDTH, IMAGE_HEIGHT]
AssignViewToLayout(renderView)

# Show the scaled data
dataDisplay = Show(transform, renderView)
dataDisplay.Representation = 'Surface With Edges'
dataDisplay.Specular = 0.5
dataDisplay.SpecularPower = 30

# Set background color
renderView.Background = [0.1, 0.2, 0.4]

# Set camera (top view)
renderView.CameraPosition = CAMERA_POSITION
renderView.CameraFocalPoint = CAMERA_FOCAL_POINT
renderView.CameraViewUp = CAMERA_VIEW_UP

if USE_PARALLEL_PROJECTION:
    renderView.CameraParallelProjection = 1
    renderView.CameraParallelScale = PARALLEL_SCALE
else:
    renderView.CameraParallelProjection = 0

Render(renderView)

# Configure animation
animationScene = GetAnimationScene()
animationScene.NumberOfFrames = len(vtk_files)
animationScene.StartTime = 0
animationScene.EndTime = len(vtk_files) - 1
animationScene.PlayMode = 'Sequence'

print(f"\nSaving {len(vtk_files)} frames to {OUTPUT_DIR}...")

# Save each frame as PNG
for i in range(len(vtk_files)):
    animationScene.TimeKeeper.Time = i
    Render()
    frame_file = os.path.join(OUTPUT_DIR, f"frame_{i+1:04d}.png")
    SaveScreenshot(frame_file, renderView, ImageResolution=[IMAGE_WIDTH, IMAGE_HEIGHT])

    if (i + 1) % 10 == 0 or (i + 1) == len(vtk_files):
        print(f"  Saved frame {i+1}/{len(vtk_files)}")

print(f"\nComplete! {len(vtk_files)} frames saved to {OUTPUT_DIR}")

# ============================================================
# Convert PNG sequence to MP4 using FFmpeg
# ============================================================
print("\n" + "=" * 50)
print("Converting PNG sequence to MP4...")
print("=" * 50)

# Find ffmpeg
ffmpeg_cmd = None
ffmpeg_paths = ['ffmpeg', 'C:\\ffmpeg\\bin\\ffmpeg.exe', 'D:\\ffmpeg\\bin\\ffmpeg.exe']

for path in ffmpeg_paths:
    try:
        subprocess.run([path, '-version'], capture_output=True, check=True)
        ffmpeg_cmd = path
        break
    except (subprocess.SubprocessError, FileNotFoundError):
        continue

if ffmpeg_cmd:
    input_pattern = os.path.join(OUTPUT_DIR, "frame_%04d.png").replace('\\', '/')
    output_video = OUTPUT_VIDEO.replace('\\', '/')

    # Use padding to fix odd height (1080 is even, but frames may be 1061)
    ffmpeg_command = [
        ffmpeg_cmd, '-framerate', str(FRAME_RATE),
        '-i', input_pattern,
        '-vf', 'pad=1920:1062:(ow-iw)/2:(oh-ih)/2',
        '-c:v', 'libx264',
        '-pix_fmt', 'yuv420p',
        '-crf', '18',
        '-y', output_video
    ]

    print("Running FFmpeg...")
    try:
        result = subprocess.run(ffmpeg_command, capture_output=True, text=True)
        if result.returncode == 0 and os.path.exists(OUTPUT_VIDEO):
            size_mb = os.path.getsize(OUTPUT_VIDEO) / (1024 * 1024)
            print(f"\nSUCCESS! MP4 created: {OUTPUT_VIDEO}")
            print(f"File size: {size_mb:.2f} MB")
        else:
            print(f"\nFFmpeg failed. Run this command manually:")
            print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')
    except Exception as e:
        print(f"\nError: {e}")
        print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')
else:
    print("\nFFmpeg not found! Install FFmpeg or run this command manually:")
    print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')

print("\nDone!")
;;;;
%slc_pvpyend;


/**************************************************************************************************************************/
/*                                                                                                                        */
/* INPUT                                                                                                                  */
/* --------------------------------------------                                                                           */
/* d:/tad                                                                                                                 */
/*   gmsh_tensile_LAW36_BIQUADA001.vtk  238,354                                                                           */
/*   gmsh_tensile_LAW36_BIQUADA002.vtk  452,231                                                                           */
/*   gmsh_tensile_LAW36_BIQUADA003.vtk  449,053                                                                           */
/* ...                                                                                                                    */
/*   ..                                                                                                                   */
/*   gmsh_tensile_LAW36_BIQUADA039.vtk  440,534                                                                           */
/*   gmsh_tensile_LAW36_BIQUADA040.vtk  439,854                                                                           */
/*                                                                                                                        */
/* OUTPUTS                                                                                                                */
/* --------------------------------------------                                                                           */
/* d:/rad/frame                                                                                                           */
/*  frame_0001.png 66KB                                                                                                   */
/*  frame_0002.png 66KB                                                                                                   */
/*  frame_0003.png 66KB                                                                                                   */
/*  ...                                                                                                                   */
/*  ...                                                                                                                   */
/*  frame_0038.png 66KB                                                                                                   */
/*  frame_0039.png 66KB                                                                                                   */
/*  frame_0040.png 66KB                                                                                                   */
/*                                                                                                                        */
/*s:/rad                                                                                                                  */
/*  animation.mp4  134KB                                                                                                  */
/*                                                                                                                        */
/*                                                                                                                        */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*
| | ___   __ _
| |/ _ \ / _` |
| | (_) | (_| |
|_|\___/ \__, |
         |___/
*/

1                                          Altair SLC          13:17 Saturday, May 16, 2026

NOTE: Copyright 2002-2025 World Programming, an Altair Company
NOTE: Altair SLC 2026 (05.26.01.00.000758)
      Licensed to Roger DeAngelis
NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
NOTE: AUTOEXEC source line
1       +  ï»¿ods _all_ close;
           ^
ERROR: Expected a statement keyword : found "?"

NOTE: AUTOEXEC processing completed

1
2         %slc_pvpybegin;
The file c:/temp/py_pgm.py does not exist
3         cards4;

NOTE: The file 'c:\temp\py_pgmx.py' is:
      Filename='c:\temp\py_pgmx.py',
      Owner Name=SLC\suzie,
      File size (bytes)=0,
      Create Time=13:21:25 Jan 12 2026,
      Last Accessed=13:17:24 May 16 2026,
      Last Modified=13:17:24 May 16 2026,
      Lrecl=32767, Recfm=V

NOTE: 162 records were written to file 'c:\temp\py_pgmx.py'
      The minimum record length was 80
      The maximum record length was 181
NOTE: The data step took :
      real time : 0.000
      cpu time  : 0.000


4         #!/usr/bin/env python
5         """
6         ParaView 6.1.0 script - Save animation as PNG sequence
7         Then convert to MP4 using FFmpeg
8         """
9
10        from paraview.simple import *
11        import glob
12        import os
13        import subprocess
14
15        # ============================================================
16        # CONFIGURATION
17        # ============================================================
18        VTK_FILE_PATTERN = "D:/rad/gmsh_tensile_LAW36_BIQUADA*.vtk"
19        OUTPUT_DIR = "D:/rad/frames"
20        OUTPUT_VIDEO = "D:/rad/animation.mp4"
21        FRAME_RATE = 10
22
23        # Image resolution (use EVEN numbers for H.264)
24        IMAGE_WIDTH = 1920
25        IMAGE_HEIGHT = 1080
26
27        # Axis scaling - make specimen wider
28        X_SCALE = 1.0
29        Y_SCALE = 1.0
30        Z_SCALE = 20.0
31
32        # Camera settings (top view)
33        CAMERA_POSITION = [0, 0, 15]
34        CAMERA_FOCAL_POINT = [0, 0, 0]
35        CAMERA_VIEW_UP = [0, 1, 0]
36        USE_PARALLEL_PROJECTION = True
37        PARALLEL_SCALE = 15.0
38
39        # Create output directory
40        os.makedirs(OUTPUT_DIR, exist_ok=True)
41
42        print("=" * 60)
43        print("ParaView 6.1.0 - Creating Animation via PNG Sequence")
44        print("=" * 60)
45        print(f"Output video: {OUTPUT_VIDEO}")
46        print(f"Frame rate: {FRAME_RATE} fps")
47        print(f"Z-axis scale: {Z_SCALE}x (wider specimen)")
48
49        # Find and load files
50        vtk_files = sorted(glob.glob(VTK_FILE_PATTERN))
51
52        if not vtk_files:
53            print(f"ERROR: No VTK files found matching {VTK_FILE_PATTERN}")
54            exit(1)
55
56        print(f"Found {len(vtk_files)} VTK files")
57
58        # Load data
59        reader = LegacyVTKReader(FileNames=vtk_files)
60
61        # Apply scaling to make specimen wider
62        transform = Transform(Input=reader)
63        transform.Transform = 'Transform'
64        transform.Transform.Scale = [X_SCALE, Y_SCALE, Z_SCALE]
65
66        # Create render view
67        renderView = CreateView('RenderView')
68        renderView.ViewSize = [IMAGE_WIDTH, IMAGE_HEIGHT]
69        AssignViewToLayout(renderView)
70
71        # Show the scaled data
72        dataDisplay = Show(transform, renderView)
73        dataDisplay.Representation = 'Surface With Edges'
74        dataDisplay.Specular = 0.5
75        dataDisplay.SpecularPower = 30
76
77        # Set background color
78        renderView.Background = [0.1, 0.2, 0.4]
79
80        # Set camera (top view)
81        renderView.CameraPosition = CAMERA_POSITION
82        renderView.CameraFocalPoint = CAMERA_FOCAL_POINT
83        renderView.CameraViewUp = CAMERA_VIEW_UP
84
85        if USE_PARALLEL_PROJECTION:
86            renderView.CameraParallelProjection = 1
87            renderView.CameraParallelScale = PARALLEL_SCALE
88        else:
89            renderView.CameraParallelProjection = 0
90
91        Render(renderView)
92
93        # Configure animation
94        animationScene = GetAnimationScene()
95        animationScene.NumberOfFrames = len(vtk_files)
96        animationScene.StartTime = 0
97        animationScene.EndTime = len(vtk_files) - 1
98        animationScene.PlayMode = 'Sequence'
99
100       print(f"\nSaving {len(vtk_files)} frames to {OUTPUT_DIR}...")
101
102       # Save each frame as PNG
103       for i in range(len(vtk_files)):
104           animationScene.TimeKeeper.Time = i
105           Render()
106           frame_file = os.path.join(OUTPUT_DIR, f"frame_{i+1:04d}.png")
107           SaveScreenshot(frame_file, renderView, ImageResolution=[IMAGE_WIDTH, IMAGE_HEIGHT])
108
109           if (i + 1) % 10 == 0 or (i + 1) == len(vtk_files):
110               print(f"  Saved frame {i+1}/{len(vtk_files)}")
111
112       print(f"\nComplete! {len(vtk_files)} frames saved to {OUTPUT_DIR}")
113
114       # ============================================================
115       # Convert PNG sequence to MP4 using FFmpeg
116       # ============================================================
117       print("\n" + "=" * 50)
118       print("Converting PNG sequence to MP4...")
119       print("=" * 50)
120
121       # Find ffmpeg
122       ffmpeg_cmd = None
123       ffmpeg_paths = ['ffmpeg', 'C:\\ffmpeg\\bin\\ffmpeg.exe', 'D:\\ffmpeg\\bin\\ffmpeg.exe']
124
125       for path in ffmpeg_paths:
126           try:
127               subprocess.run([path, '-version'], capture_output=True, check=True)
128               ffmpeg_cmd = path
129               break
130           except (subprocess.SubprocessError, FileNotFoundError):
131               continue
132
133       if ffmpeg_cmd:
134           input_pattern = os.path.join(OUTPUT_DIR, "frame_%04d.png").replace('\\', '/')
135           output_video = OUTPUT_VIDEO.replace('\\', '/')
136
137           # Use padding to fix odd height (1080 is even, but frames may be 1061)
138           ffmpeg_command = [
139               ffmpeg_cmd, '-framerate', str(FRAME_RATE),
140               '-i', input_pattern,
141               '-vf', 'pad=1920:1062:(ow-iw)/2:(oh-ih)/2',
142               '-c:v', 'libx264',
143               '-pix_fmt', 'yuv420p',
144               '-crf', '18',
145               '-y', output_video
146           ]
147
148           print("Running FFmpeg...")
149           try:
150               result = subprocess.run(ffmpeg_command, capture_output=True, text=True)
151               if result.returncode == 0 and os.path.exists(OUTPUT_VIDEO):
152                   size_mb = os.path.getsize(OUTPUT_VIDEO) / (1024 * 1024)
153                   print(f"\nSUCCESS! MP4 created: {OUTPUT_VIDEO}")
154                   print(f"File size: {size_mb:.2f} MB")
155               else:
156                   print(f"\nFFmpeg failed. Run this command manually:")
157                   print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')
158           except Exception as e:
159               print(f"\nError: {e}")
160               print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')
161       else:
162           print("\nFFmpeg not found! Install FFmpeg or run this command manually:")
163           print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')
164
165       print("\nDone!")
166       ;;;;
167       %slc_pvpyend;

NOTE: The infile 'c:\temp\py_pgmx.py' is:
      Filename='c:\temp\py_pgmx.py',
      Owner Name=SLC\suzie,
      File size (bytes)=13590,
      Create Time=13:21:25 Jan 12 2026,
      Last Accessed=13:17:24 May 16 2026,
      Last Modified=13:17:24 May 16 2026,
      Lrecl=32767, Recfm=V

NOTE: The file 'c:\temp\py_pgm.py' is:
      Filename='c:\temp\py_pgm.py',
      Owner Name=SLC\suzie,
      File size (bytes)=0,
      Create Time=16:38:15 May 12 2026,
      Last Accessed=13:17:24 May 16 2026,
      Last Modified=13:17:24 May 16 2026,
      Lrecl=32767, Recfm=V

#!/usr/bin/env python
"""
ParaView 6.1.0 script - Save animation as PNG sequence
Then convert to MP4 using FFmpeg
"""

from paraview.simple import *
import glob
import os
import subprocess

# ============================================================
# CONFIGURATION
# ============================================================
VTK_FILE_PATTERN = "D:/rad/gmsh_tensile_LAW36_BIQUADA*.vtk"
OUTPUT_DIR = "D:/rad/frames"
OUTPUT_VIDEO = "D:/rad/animation.mp4"
FRAME_RATE = 10

# Image resolution (use EVEN numbers for H.264)
IMAGE_WIDTH = 1920
IMAGE_HEIGHT = 1080

# Axis scaling - make specimen wider
X_SCALE = 1.0
Y_SCALE = 1.0
Z_SCALE = 20.0

# Camera settings (top view)
CAMERA_POSITION = [0, 0, 15]
CAMERA_FOCAL_POINT = [0, 0, 0]
CAMERA_VIEW_UP = [0, 1, 0]
USE_PARALLEL_PROJECTION = True
PARALLEL_SCALE = 15.0

# Create output directory
os.makedirs(OUTPUT_DIR, exist_ok=True)

print("=" * 60)
print("ParaView 6.1.0 - Creating Animation via PNG Sequence")
print("=" * 60)
print(f"Output video: {OUTPUT_VIDEO}")
print(f"Frame rate: {FRAME_RATE} fps")
print(f"Z-axis scale: {Z_SCALE}x (wider specimen)")

# Find and load files
vtk_files = sorted(glob.glob(VTK_FILE_PATTERN))

if not vtk_files:
    print(f"ERROR: No VTK files found matching {VTK_FILE_PATTERN}")
    exit(1)

print(f"Found {len(vtk_files)} VTK files")

# Load data
reader = LegacyVTKReader(FileNames=vtk_files)

# Apply scaling to make specimen wider
transform = Transform(Input=reader)
transform.Transform = 'Transform'
transform.Transform.Scale = [X_SCALE, Y_SCALE, Z_SCALE]

# Create render view
renderView = CreateView('RenderView')
renderView.ViewSize = [IMAGE_WIDTH, IMAGE_HEIGHT]
AssignViewToLayout(renderView)

# Show the scaled data
dataDisplay = Show(transform, renderView)
dataDisplay.Representation = 'Surface With Edges'
dataDisplay.Specular = 0.5
dataDisplay.SpecularPower = 30

# Set background color
renderView.Background = [0.1, 0.2, 0.4]

# Set camera (top view)
renderView.CameraPosition = CAMERA_POSITION
renderView.CameraFocalPoint = CAMERA_FOCAL_POINT
renderView.CameraViewUp = CAMERA_VIEW_UP

if USE_PARALLEL_PROJECTION:
    renderView.CameraParallelProjection = 1
    renderView.CameraParallelScale = PARALLEL_SCALE
else:
    renderView.CameraParallelProjection = 0

Render(renderView)

# Configure animation
animationScene = GetAnimationScene()
animationScene.NumberOfFrames = len(vtk_files)
animationScene.StartTime = 0
animationScene.EndTime = len(vtk_files) - 1
animationScene.PlayMode = 'Sequence'

print(f"\nSaving {len(vtk_files)} frames to {OUTPUT_DIR}...")

# Save each frame as PNG
for i in range(len(vtk_files)):
    animationScene.TimeKeeper.Time = i
    Render()
    frame_file = os.path.join(OUTPUT_DIR, f"frame_{i+1:04d}.png")
    SaveScreenshot(frame_file, renderView, ImageResolution=[IMAGE_WIDTH, IMAGE_HEIGHT])

    if (i + 1) % 10 == 0 or (i + 1) == len(vtk_files):
        print(f"  Saved frame {i+1}/{len(vtk_files)}")

print(f"\nComplete! {len(vtk_files)} frames saved to {OUTPUT_DIR}")

# ============================================================
# Convert PNG sequence to MP4 using FFmpeg
# ============================================================
print("\n" + "=" * 50)
print("Converting PNG sequence to MP4...")
print("=" * 50)

# Find ffmpeg
ffmpeg_cmd = None
ffmpeg_paths = ['ffmpeg', 'C:\\ffmpeg\\bin\\ffmpeg.exe', 'D:\\ffmpeg\\bin\\ffmpeg.exe']

for path in ffmpeg_paths:
    try:
        subprocess.run([path, '-version'], capture_output=True, check=True)
        ffmpeg_cmd = path
        break
    except (subprocess.SubprocessError, FileNotFoundError):
        continue

if ffmpeg_cmd:
    input_pattern = os.path.join(OUTPUT_DIR, "frame_%04d.png").replace('\\', '/')
    output_video = OUTPUT_VIDEO.replace('\\', '/')

    # Use padding to fix odd height (1080 is even, but frames may be 1061)
    ffmpeg_command = [
        ffmpeg_cmd, '-framerate', str(FRAME_RATE),
        '-i', input_pattern,
        '-vf', 'pad=1920:1062:(ow-iw)/2:(oh-ih)/2',
        '-c:v', 'libx264',
        '-pix_fmt', 'yuv420p',
        '-crf', '18',
        '-y', output_video
    ]

    print("Running FFmpeg...")
    try:
        result = subprocess.run(ffmpeg_command, capture_output=True, text=True)
        if result.returncode == 0 and os.path.exists(OUTPUT_VIDEO):
            size_mb = os.path.getsize(OUTPUT_VIDEO) / (1024 * 1024)
            print(f"\nSUCCESS! MP4 created: {OUTPUT_VIDEO}")
            print(f"File size: {size_mb:.2f} MB")
        else:
            print(f"\nFFmpeg failed. Run this command manually:")
            print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')
    except Exception as e:
        print(f"\nError: {e}")
        print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')
else:
    print("\nFFmpeg not found! Install FFmpeg or run this command manually:")
    print(f'ffmpeg -framerate {FRAME_RATE} -i "{OUTPUT_DIR}/frame_%04d.png" -vf "pad=1920:1062:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p -crf 18 -y {OUTPUT_VIDEO}')

print("\nDone!")
NOTE: 162 records were read from file 'c:\temp\py_pgmx.py'
      The minimum record length was 80
      The maximum record length was 181
NOTE: 162 records were written to file 'c:\temp\py_pgm.py'
      The minimum record length was 80
      The maximum record length was 181
NOTE: The data step took :
      real time : 0.000
      cpu time  : 0.015



NOTE: The infile rut is:
      Unnamed Pipe Access Device,
      Process=C:\Progra~1\ParaView-6.1.0-Windows-Python3.12-msvc2017-AMD64\bin\pvpython.exe c:/temp/py_pgm.py 2> c:/temp/py_pgm.log,
      Lrecl=32767, Recfm=V

============================================================
ParaView 6.1.0 - Creating Animation via PNG Sequence
============================================================
Output video: D:/rad/animation.mp4
Frame rate: 10 fps
Z-axis scale: 20.0x (wider specimen)
Found 41 VTK files

Saving 41 frames to D:/rad/frames...
  Saved frame 10/41
  Saved frame 20/41
  Saved frame 30/41
  Saved frame 40/41
  Saved frame 41/41

Complete! 41 frames saved to D:/rad/frames

==================================================
Converting PNG sequence to MP4...
==================================================
Running FFmpeg...

SUCCESS! MP4 created: D:/rad/animation.mp4
File size: 0.13 MB

Done!
NOTE: 26 records were written to file PRINT

NOTE: 26 records were read from file rut
      The minimum record length was 0
      The maximum record length was 60
NOTE: The data step took :
      real time : 15.741
      cpu time  : 0.000



NOTE: The infile 'c:\temp\py_pgm.log' is:
      Filename='c:\temp\py_pgm.log',
      Owner Name=SLC\suzie,
      File size (bytes)=0,
      Create Time=11:45:37 May 12 2026,
      Last Accessed=13:17:24 May 16 2026,
      Last Modified=13:17:24 May 16 2026,
      Lrecl=32767, Recfm=V

NOTE: No records were read from file 'c:\temp\py_pgm.log'
NOTE: The data step took :
      real time : 0.000
      cpu time  : 0.000


168
ERROR: Error printed on page 1

NOTE: Submitted statements took :
      real time : 15.884
      cpu time  : 0.156

/*              _
  ___ _ __   __| |
 / _ \ `_ \ / _` |
|  __/ | | | (_| |
 \___|_| |_|\__,_|

*/
