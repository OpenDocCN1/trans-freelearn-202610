5. It is now time to find IPRIPa.dll in memory. Open the memory window of OllyDbg (View | Memory or Alt + M). Now scroll through the output of the memory window and find the .text memory section (the executable part of the dll) for IPRIPa.dll:

![](bbox=[0.11, 0.19, 0.85, 0.39])

Figure 11.50 – OllyDbg – svchost memory modules

6. Right-click on the section and choose View in Disassembler to direct the disassembler output to the beginning of the text section and the GUI to switch to the disassembler view:

![](bbox=[0.11, 0.5, 0.85, 0.8])

Figure 11.51 – OllyDbg – ipripa.dll start