# Home Printing Instructions

This repository contains 3D print files and settings for home printing. Use this README as a quick guide for slicing and printing the included models.

## Quick start
1. Open the desired model (.stl/.3mf) in your slicer (Cura, PrusaSlicer, Slic3r, Simplify3D).
2. Choose the material and printer profile closest to your hardware.
3. Apply the settings below as a baseline and adjust for your printer.

## Recommended baseline settings (3D printing)
- Layer height: 0.2 mm (0.12–0.3 mm depending on detail vs speed)
- Nozzle: 0.4 mm
- Print speed: 40–60 mm/s (lower for small detailed parts)
- Infill: 15–25% (20% is a good general-purpose value)
- Walls/perimeters: 2–3
- Top solid layers: 4–6
- Bottom solid layers: 4–6
- Retraction: enable (start: 1 mm, speed: 25–45 mm/s) — tune for Bowden vs direct drive

## Temperature (common filaments)
- PLA: nozzle 200°C, bed 50–60°C
- PETG: nozzle 235–250°C, bed 70–85°C
- ABS: nozzle 230–250°C, bed 90–110°C (use enclosure)
- TPU: nozzle 210–230°C, bed 30–50°C (slow speeds, direct drive recommended)

Adjust temperatures based on manufacturer filament datasheet and your printer's calibration.

## Bed adhesion (3D)
- Use a clean, leveled bed. Re-level if you change nozzle or remove/replace the bed surface.
- Use glue stick, blue painter's tape, PEI, or BuildTak as appropriate.
- Brims for small bases or thin parts; rafts if the model warps or requires stronger adhesion.

## Supports and orientation (3D)
- Orient parts to minimize overhangs and supports where practical.
- Use supports for overhangs > 60° or where small features would droop.
- For cosmetic surfaces, use tree supports or lower support density and increase support Z-distance.

## Post-processing (3D)
- Remove supports carefully with flush cutters or pliers.
- Sanding: start with 200–400 grit and move to 800–2000 for smooth finish.
- For PLA smoothing, consider light sanding and polishing; for ABS acetone vapor smoothing works (use caution).

## Troubleshooting (3D)
- First layer not sticking: re-level bed, increase first-layer temperature, slow down first layer, increase first-layer height/flow.
- Stringing: increase retraction distance/speed, lower print temperature.
- Warping: increase bed temp, add enclosure, use brim/raft.
- Elephant's foot: reduce first layer Z offset, or add a skirt only.

---

## 2D Document Printing on Linux (CUPS)
This section covers common settings and steps for printing documents (PDFs, text, images) to network or USB printers on Linux using CUPS. It includes specific guidance for the Brother HL-2270dw and generic Canon printers.

Prerequisites
- CUPS installed and running (on Debian/Ubuntu: `sudo apt install cups` and enable/start with `sudo systemctl enable --now cups`).
- Your user should be able to use printers: add to groups if needed (`sudo usermod -aG lp $(whoami)`), and log out/in.
- For network discovery, ensure Avahi/mDNS is enabled if you want automatic discovery (`sudo apt install avahi-daemon`).

General CUPS tips
- Access the CUPS web interface at: http://localhost:631
- Add a printer via that web UI or `lpadmin` from the command line.
- Common print options (lp/lpr/CUPS):
  - sides=one-sided | two-sided-long-edge | two-sided-short-edge
  - media=Letter | A4 | Legal
  - resolution=300dpi | 600dpi
  - number-up=1 | 2 | 4

Commands
- List printers and default: `lpstat -p -d`
- Print a PDF with duplex long-edge: `lp -d PRINTER_NAME -o sides=two-sided-long-edge -o media=Letter -o resolution=600dpi file.pdf`
- Set printer defaults: `lpadmin -p PRINTER_NAME -o sides=two-sided-long-edge -o PageSize=Letter -E`

### Brother HL-2270dw (Linux-specific notes)
The Brother HL-2270dw is a monochrome laser printer with optional network (wireless) and duplex capability. On Linux, you can install either the official Brother drivers or use open-source drivers where available.

Driver installation (recommended options)
- Option A — brlaser (open-source):
  - On Debian/Ubuntu: `sudo apt install printer-driver-brlaser`
  - This driver supports many Brother laser printers and often works without the official wrapper.
- Option B — Brother official drivers:
  - Download from Brother support: https://support.brother.com
  - Look for the "Linux (deb/rpm)" cupswrapper and lpr packages for HL-2270DW.
  - Install the `lpr` package first, then the `cupswrapper` package (follow Brother's README).

Add the printer
- For network (recommended static IP or DHCP-assigned IP):
  - Use socket/JetDirect: socket://192.168.1.45 (replace with the printer IP)
  - Or IPP: ipp://192.168.1.45/ipp/print
- For USB: select the detected usb://... URI in CUPS or use `usb://Brother/HL-2270DW`.
- Choose the HL-2270DW PPD from the installed driver package or select the brlaser driver.

Recommended print options for text documents
- Page size: Letter or A4 (set according to your region)
- Resolution: 600dpi (`-o resolution=600dpi`) for sharp text
- Duplex: enable two-sided (`-o sides=two-sided-long-edge`) — HL-2270dw supports duplexing
- Toner save: off for best quality (option name may vary by driver/PPD)

Example print command
- Single-sided, A4, 600 dpi: `lp -d Brother_HL2270DW -o sides=one-sided -o media=A4 -o resolution=600dpi document.pdf`
- Duplex long-edge Letter: `lp -d Brother_HL2270DW -o sides=two-sided-long-edge -o media=Letter -o resolution=600dpi document.pdf`

Troubleshooting (Brother)
- Printer not found: check IP, firewall (port 631 or JetDirect 9100), and make sure printer is on same network.
- Permission denied: ensure `cups` is running and your user is in the `lp`/`lpadmin` group.
- Driver missing: install brlaser or Brother's cupswrapper package and restart CUPS (`sudo systemctl restart cups`).

### Canon printers (generic guidance)
Canon has many models with different driver support. The steps below are for typical Canon laser/inkjet models on Linux.

Driver options
- Gutenprint (open-source) supports many Canon consumer inkjets: `sudo apt install printer-driver-gutenprint`
- Canon's official drivers (cnijfilter or UFRII): available from Canon download pages (look for packages named `cnijfilter` or `cndrvcups-ufr2` for UFR-II printers).

Add the printer
- Use the CUPS web UI or `lpadmin`. For network printers, use socket/ipp URIs (e.g., `socket://192.168.1.50` or `ipp://192.168.1.50/ipp/print`). For USB, select the usb URI.
- Choose the appropriate Canon PPD from Gutenprint or the Canon driver package.

Recommended options (documents)
- Media/page size: A4 or Letter (`-o media=A4`)
- Resolution: 300–600 dpi (`-o resolution=300dpi` or `-o resolution=600dpi`)
- Color printing: `-o ColorModel=RGB` or driver-specific option (may be `ColorModel=Gray` for grayscale)
- Duplex: `-o sides=two-sided-long-edge` if supported

Example commands
- Basic print: `lp -d Canon_Printer -o media=A4 -o resolution=300dpi file.pdf`
- Photo/graphics (higher quality): `lp -d Canon_Printer -o media=Photo -o resolution=600dpi file.jpg`

Troubleshooting (Canon)
- If the driver isn't available, try Gutenprint; if that fails, look for the official Canon package for your model.
- Color looks wrong: try switching ColorModel or ensure paper type matches driver setting.
- Print jobs stuck: check `lpstat -W completed` and `cancel -a` to clear jobs, and restart CUPS if needed.

---

## Licensing and credits
Check each model's header or the repository LICENSE file for licensing terms. Credit original designers where applicable.

## Contact / Feedback
If you find issues with prints or need recommended settings for a specific printer, open an issue in this repository with details (printer model, filament/paper, symptom, and pictures/logs if possible).
