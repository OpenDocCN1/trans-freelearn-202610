# # Mergecap batch file

If the capture files you want to merge have a variety of naming formats, you can create a `MergeTraces.bat` file containing the following Windows batch commands:

```
```bat
@echo off
cls
echo MergeTraces.bat
echo.
echo Merges multiple packet trace files with a .pcapng extension into one .pcapng file
echo.
echo Usage: Copy MergeTraces.bat into the directory with the .pkt files and execute
echo The utility will generate a 'MergedTraces.pcap' file
echo and a 'MergedFileList.txt' file which lists the .pcapng files processed.
echo.
echo.
echo IMPORTANT!! You must type 'CMD /V:ON' from this window which enables
echo 'Delayed environment variable expansion' in order to properly execute
echo this batch utility.
echo.
echo You must also add the path to Wireshark's mergecap.exe to your path statement.
echo.
echo If you've not done this, Type Ctrl-C to exit; Otherwise pause
echo.
echo Deleting old MergedFileList.txt...
if exist "MergedFileList.txt" del MergedFileList.txt
for %%f in (*.pcap-ng) do echo "%%f" >> MergedFileList.txt
echo Deleting old MergedTraces.pcapng...
if exist "MergedTraces.pcapng" del MergedTraces.pcapng
echo Preparing to merge:
echo.
type MergedFileList.txt
echo.
echo Merging..........
set FILELIST=
for %%f in (*.pcap-ng) do set FILELIST=!FILELIST! %%f
:: DEBUG
:: echo %FILELIST%
```
```