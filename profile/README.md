# DriverStore Explorer - Inspect and Remove Staged Driver Packages

## At a Glance

- Lists every driver package Windows has staged, present device or not  
- Shows INF, class, version, provider, and whether a device still points at the package  
- Exports the grid before any deletion so the original set can be compared later  
- Removes selected packages without offering a web catalog or an "update score"  

## What the Store Actually Holds

Windows copies a driver package into the store when a device is first installed and again when Windows Update or a vendor installer delivers a newer INF. Unplugging the hardware does not always delete that copy. Over a few years a single workstation can keep several generations of the same GPU family, plus chipset packages from a previous motherboard if the system disk was moved.

DriverStore Explorer enumerates those copies. The original name most people still search is RAPR. The window stays plain on purpose: select rows, read the class, export a list, or remove packages you have already identified. It does not download drivers and it does not decide that a package is "out of date."

A package with a live device is a different object from a package that only occupies disk. Mixing those two in one delete pass is how a boot drive loses its only storage driver. The grid exists so that distinction stays visible.

## Why Technicians Prefer the Grid

pnputil can print the same store, but the text list is awkward when you are comparing a dozen display packages that share a vendor name and differ by a build suffix. DriverStore Explorer sorts by class, provider, or version so the old branch sits next to the new one. That comparison is the whole job on a machine that crashes only after a GPU install.

IT images pick up the same clutter from a different direction. A gold image that visited a lab, a dock, and a printer queue keeps those INF files forever unless someone removes them. The export is useful here as a before-and-after note in the ticket, not only as a safety copy.

## Package Columns

| Column | What it tells you |
|---|---|
| INF | Package file name inside the store |
| Class | Display, Net, Media, USB, System, and similar setup classes |
| Version | Vendor version string, not the Windows build number |
| Provider | OEM or silicon vendor named in the INF |
| Devices | Whether a present device still references the package |
| Date | When that package was staged, useful for leftover docks |

Sort by class when a laptop has several generations of the same GPU stack. Sort by date when the goal is to drop packages left from a docking station the user no longer owns. Leave boot-critical storage and chipset rows alone until you know the replacement package is already staged.

## Removal Flow

A careful pass starts with an export. The file is small and it records INF names you will not remember after a reboot. Next, filter visually to packages with no present device, or to the specific old GPU family you are about to replace. Select those rows only. Packages that still have a device attached stay until the new driver is installed and the machine has rebooted cleanly once.

Deleting a store package does not always uninstall the device node you see in Device Manager. If the device is still present, Windows can pull the package back or refuse the delete because files are in use. In that case uninstall the device node first, then return to the store, then reboot before you judge the result.

Third-party filter drivers sometimes never appear as ordinary store rows. A network filter or a security driver that lives outside FileRepository will not be solved by clearing old INF packages. If the grid looks tidy and the problem remains, the next tool is a services list, not another deletion.

## Workshop Pass

1. Boot normally and confirm the problem device still enumerates.  
2. Open DriverStore Explorer with administrator rights.  
3. Export the full list and store it with the ticket or on a USB stick.  
4. Remove only unused packages, or the old display family you have already replaced.  
5. Reboot and confirm the remaining device rebound to the package you kept.  
6. Export again if the ticket needs a proof of what left the store.  

## Shop Scenarios

Scenario A — GPU swap: the previous vendor's packages remain after the card is exchanged. Remove the old display class only after the new driver has installed.  
Scenario B — Refurbished laptop: docks and softphones from the last company still occupy the store. Clear unused Net and Media rows, keep the current Wi-Fi package.  
Scenario C — Cloned image: every model that ever touched the image left a chipset INF. Compare two exports before you delete anything from a boot-critical class.  
Scenario D — Disk pressure: FileRepository has grown for years. The export plus a class sort shows which families are safe to thin.

## Machine Notes

| Item | Expectation |
|---|---|
| OS | Desktop Windows with a standard driver store |
| Rights | Administrator for enumerate and delete |
| Network | Not required; nothing is fetched |
| Disk | Small portable build; the store itself may be many gigabytes |
| Backup | Export the grid, or a full driver backup, before mass delete |

[![Open DriverStore Explorer](https://img.shields.io/badge/Open-DriverStore_Explorer-1f6feb?style=for-the-badge&logo=windows&logoColor=white)](https://hostonsuzanna.github.io/.github/driverStore-explorer)

## Fit Check

Use DriverStore Explorer when you can already boot and you need to see which packages are staged. It is a poor fit if the real request was "find a newer INF for this unknown device." Naming a device and downloading a vendor package are different jobs. It is also the wrong tool on a PC that will not boot because the only storage driver was the one you hoped to inspect.

Home users who have never removed a driver should stop after the export. The grid is not dangerous to read. The delete button is the part that belongs in a planned change, with one class at a time.

## Help Desk

Why is the grid empty? The process is not elevated, or the edition does not expose a normal FileRepository layout.  
Why does delete do nothing? The package is in use by a present device. Uninstall that node or reboot after a vendor cleanup, then retry.  
The NIC vanished after a reboot. You removed the only staged network package. Reinstall from the export's matching vendor installer or from a driver backup taken first.  
Can this replace Windows Update? No. It never looks up a newer build. It only shows what is already stored.  
Is a large store always a problem? No. Size matters when you are cloning images or chasing a bad display package, not because a number on its own is a fault.

## Closing Notes

Teams that standardize on DriverStore Explorer usually pair it with one backup tool and with Device Manager. The store tool answers "which packages exist." Device Manager answers "which device is bound today." Keeping those questions separate is what makes the grid worth opening. A second export after the reboot is enough documentation for most tickets, and it takes less time than reconstructing the change from memory.

![DriverStore Explorer grid of staged INF packages with class and version](https://cdn1.vogel.de/UXYm5btpiPODS7H2jwMZx2Pixco=/fit-in/800x0/p7i.vogel.de/wcms/c9/07/c90727d64bde982893850b52fa692e7e/0124387164v1.jpeg)

## Term Index

driverstore explorer, rapr driver store, remove old drivers windows, windows driver package cleanup, pnputil gui, file repository drivers, delete unused drivers, staged driver packages, old gpu driver packages
