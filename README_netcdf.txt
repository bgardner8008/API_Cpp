
How to build static Windows libs for netcdf. By default the libs that come with libsofa are DLL import libs and we want to avoid shipping DLLs with plugs.

1) Prereqs: Install Cmake. Install M4.exe and regex2.dll to c:\bin or some other directory which is on PATH.

2) Install netcdf-c, easiest to clone from https://github.com/Unidata/netcdf-c.git

3) Configure netcdf with CMake

E:\SOFA\netcdf-c>del CMakeCache.txt
E:\SOFA\netcdf-c>cmake -DENABLE_NETCDF_4=OFF -DENABLE_DAP=OFF -DBUILD_UTILITIES=OFF -DENABLE_TESTS=OFF -DENABLE_HDF5=OFF -DBUILD_SHARED_LIBS=OFF .

4) Build with Visual Studio, open netCDF.sln, select Release, and rebuild netcdf project

It makes a static lib:
6>netcdf.vcxproj -> E:\SOFA\netcdf-c\liblib\Release\netcdf.lib

Copy this .lib to to E:\SOFA\API_Cpp\libsofa\dependencies\lib\win
overwriting the DLL Import lib, rebuild sofainfo, and it works!!!

Static netcdf lib on Windows had been achieved.

Still need to build 64-bit as well. Is this another CMake option?
Yes, add "-A x64" to cmake options and reload VS project and build, this creates 64-bit lib:

6>------ Rebuild All started: Project: netcdf, Configuration: Release x64 ------
...
6>netcdf.vcxproj -> E:\work\SOFA-code\netcdf-c\liblib\Release\netcdf.lib

rename this to netcdf_x64.lib and copy to API_Cpp\libsofa\dependencies\lib\win\netcdf_x64.lib

If we ever need to debug, we'd have to build the debug config, copy the .lib and .pdb and then rebuild and debug project.
