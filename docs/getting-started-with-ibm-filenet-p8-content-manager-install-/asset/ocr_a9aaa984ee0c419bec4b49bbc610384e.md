# Support, Fix Packs, and Troubleshooting

Our methodology for installing updates to this supporting software was to do it in the order described above, but also to reboot the RHEL server after installing each item and run a few tests to make sure we hadn't made any blunders. Whenever we are upgrading an environmental component, we stop all of the CM components first so that things are as idle as possible. The reboots and testing are theoretically not needed, but we wanted to have an easier time pinpointing a cause if something went wrong.

# CM components

Different FileNet-provided components of CM have their own fix pack cycles and fix pack installers. We will upgrade our installation with the latest fix packs for all of the components that we have installed. (The PE didn't happen to have any fix packs for PE 4.5.1, but it did have some interim fixes available. We will not be installing the PE interim fixes here, but you can download and install them at your own discretion.) The resulting version numbers for individual components are summarized in this table:

| Component | Fix Pack Level |
|---|---|
| CE | 4.5.1.4 |
| CSE | 4.5.1.1 |
| XT | 1.1.4.8 |

For CM server components, there is usually a server fix pack installer and a client fix pack installer, issued simultaneously. We will use both in appropriate places to update related components. The idea of the separate installers is that the client-side API components can be kept in sync with the updated server components.

> The CE 5.0.0 release includes a feature called Downloadable Client Components. Using a small and stable API, client applications can obtain CE JAR files and other artifacts directly from the running CE server. That's a good way for applications to ensure they always have client pieces matching the CE server. However, because it's a new feature, it will be a while before most applications have adopted it.

The first step in planning for CM fix pack installation is to make sure that all of the contemplated fix packs are compatible, and that means checking the *FileNet P8 Fix Pack Compatibility Matrices* mentioned previously. Be sure to get a fresh copy of the spreadsheet since it is frequently updated. If you are installing a recently released fix pack, it might not yet be mentioned in the matrix. Most combinations of “the latest stuff” will be compatible, but you can always check with IBM support if there is doubt after you've checked the spreadsheet.