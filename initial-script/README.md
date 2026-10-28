# Initial script — pure Python, no PyLabRobot

This folder preserves the **first working Python driver** for the Thermo Fisher NanoDrop 1000: the code written straight after reverse-engineering the USB protocol, before the project moved to [PyLabRobot](https://github.com/PyLabRobot/pylabrobot). It talks to the instrument directly through [`PyUSB`](https://github.com/pyusb/pyusb) — no PyLabRobot, no vendor software.

The code is forked from the `Code/` folder of **[Jordvl/Pylabrobot-Open-science-ND1000](https://github.com/Jordvl/Pylabrobot-Open-science-ND1000/tree/main/Code)**, the original open-science ND-1000 repository.

## Files

| File | What it is |
|---|---|
| [`nanodrop.py`](nanodrop.py) | The `NanoDrop1000` driver class: USB endpoints, wake-up, wavelength calibration from the EEPROM, interleaved 12-bit pixel decoding, lamp and solenoid-clamp control, blanking, and Beer–Lambert absorbance. |
| [`main.py`](main.py) | Interactive command-line demo: connect, take a water blank, measure a sample, and plot absorbance with markers at 260 nm (DNA) and 280 nm (protein). |
| [`ND1000 TEST.ipynb`](ND1000%20TEST.ipynb) | Exploration notebook used while decoding the device — USB enumeration, first spectra, and protocol experiments. |
| [`NanoDropInstalInstructions.txt`](NanoDropInstalInstructions.txt) | Step-by-step Windows guide: BOS-descriptor registry fix, installing `libusb-win32` with Zadig, and reverting to the official driver. |
| [`bosdescriptor.reg`](bosdescriptor.reg) | Registry fix that makes Windows 8/10/11 skip the USB BOS-descriptor query for the NanoDrop's USB 1.1 microcontroller. |
| [`pylabrobot-prototype/ThermoFisherNanoDrop1000_PLR_V1.py`](pylabrobot-prototype/ThermoFisherNanoDrop1000_PLR_V1.py) | First PyLabRobot port of the driver (early prototype), kept for provenance. The maintained implementation lives upstream in [PyLabRobot PR #1166](https://github.com/PyLabRobot/pylabrobot/pull/1166). |

## Requirements

```
pip install pyusb numpy matplotlib
```

On Windows you also need the `libusb-win32` driver bound to the NanoDrop (see [`NanoDropInstalInstructions.txt`](NanoDropInstalInstructions.txt)) and, on Windows 8 and newer, the BOS-descriptor registry fix ([`bosdescriptor.reg`](bosdescriptor.reg)).

## Usage

```bash
python main.py
```

The script connects to the instrument (USB `2457:1002`), downloads the factory calibration coefficients, then asks you to place a blank on the pedestal, followed by your sample. It prints the wavelength calibration and plots the measured absorbance between 220 and 750 nm.

## Notes

- This is **research-grade prototype code**, kept as a historical record of the reverse-engineering step. For production use, use the PyLabRobot backend in [`../demonstration/`](../demonstration/) or upstream PyLabRobot.
- The absorbance calculation follows `A = −log10((sample − dark) / (blank − dark))`, with clipping to avoid `log(0)` from camera noise.
- The same code is the basis of the technical description in the [full report](../reports/OSE_NanoDrop_Report.docx).

## Credit and license

Original reverse engineering and code: **Jord van Leeuwen** ([Jordvl/Pylabrobot-Open-science-ND1000](https://github.com/Jordvl/Pylabrobot-Open-science-ND1000)). This fork is distributed under the repository's [MIT license](../LICENSE).

[← Back to the main README](../README.md)
