# Home Printing Instructions

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
  - Use socket Brother: ' socket://192.168.100.17:9100 '
- Choose the HL-2270DW PPD from the installed driver package or select the brlaser driver.

Recommended print options for text documents
- Page size: Letter or A4 (set according to your region)
- Resolution: 600dpi (`-o resolution=600dpi`) for sharp text
- Duplex: enable two-sided (`-o sides=two-sided-long-edge`) — HL-2270dw supports duplexing
- Toner save: off for best quality (option name may vary by driver/PPD)


### Canon printers (generic guidance)
Canon has many models with different driver support. The steps below are for typical Canon laser/inkjet models on Linux.

Driver options
- Gutenprint (open-source) supports many Canon consumer inkjets: `sudo apt install printer-driver-gutenprint`
- Canon's official drivers (cnijfilter or UFRII): available from Canon download pages (look for packages named `cnijfilter` or `cndrvcups-ufr2` for UFR-II printers).

Add the printer
- Canon 'ipp://192.168.100.42/ipp/print'
- Use the CUPS web UI or `lpadmin`. For network printers, use socket/ipp URIs (e.g., `ipp://192.168.100.42/ipp/print`). For USB, select the usb URI.
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
