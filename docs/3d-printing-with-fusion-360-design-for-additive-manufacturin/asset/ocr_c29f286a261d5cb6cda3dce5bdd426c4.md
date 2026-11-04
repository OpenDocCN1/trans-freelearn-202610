| Fused Filament Fabrication | Directed Energy Deposition | Binder Jetting | Metal Powder Bed Fusion |
|---|---|---|---|
|  |  |  | Renishaw |
|  |  |  | SLM Solutions |
|  |  |  | Xact Metal |

Table 12.1 – Fusion 360 supported machine manufacturers for metal 3D printing

In Table 12.1, you will notice that certain manufacturers have an * next to them. The Fusion 360 machine library includes machines from all of the machine manufacturers listed in this table. However, you may not be able to use Fusion 360 to slice your models and create the additive toolpath to 3D print them using a metal filament or metal powder for all machines in Machine Library.

For machines from manufacturers with an *, Fusion 360 may not have the necessary print setting in its library or may not be able to create the specific slicing file format required by the machine maker. You can, however, use Fusion 360 to orient your parts within the printer’s build volume and generate support structures as needed.

Both UltiMaker and Raise3D utilize FFF technology to 3D print with metal filaments. If you have an UltiMaker S5 (or Method 3D) printer with the metal expansion kit, you can upgrade your 3D printer to unlock metal FFF capabilities. Using this upgrade, you will be able to print using BASF Ultrafuse 17-4PH or 316L material. The necessary print settings for these materials can be downloaded from UltiMaker Cura software’s Material Marketplace. Once downloaded, you can use them with cc0.4 or cc0.6 cores, as shown in Figure 12.2. The Fusion 360 print setting library does not include a validated print setting for these metal filaments with UltiMaker printers:

![](bbox=[0.13, 0.58, 0.85, 0.9])

Figure 12.2 – UltiMaker S5 with metal expansion kit and filaments from BASF