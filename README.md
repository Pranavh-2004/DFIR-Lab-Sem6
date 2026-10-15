# UE23CS342BA6 : Digital Forensics and Incident Response Laboratory

This repository contains lab reports, analysis write-ups, exercise notes, and problem statements for the laboratory exercises, activities, and CTF challenges completed during the sixth semester as part of the **Digital Forensics and Incident Response (DFIR)** course.

## Repository Overview

The repository is organised into lab folders, activity folders, and a CTF collection. It holds the written work and reference material for each exercise; the bulky forensic evidence (disk/memory images, media, browser-cache dumps, registry hives) and the third-party tool installers used during the labs are **not** tracked — see [Notes on Contents](#notes-on-contents).

### Labs

- **Lab1** — Forensic Imaging: disk acquisition with `dd`, FTK Imager, and Guymager.
- **Lab2** — Autopsy & Volatility: file-system investigation and memory-dump analysis.
- **Lab3** — Deleted Data Recovery & Metadata: carving with PhotoRec/Recuva (3A) and EXIF/metadata analysis with ExifTool (3B).
- **Lab4** — Windows Registry Forensics: analysis of registry hives with RegRipper and Registry Explorer.
- **Lab5** — Network Forensics: packet-capture analysis with Wireshark.
- **Lab6** — Browser Forensics: recovering history, cache, and artifacts from Chrome and Firefox.
- **Lab7** — Email Forensics: analysis of mail stores and `.eml` messages.

### Activities

- **Activity 1** — File signatures, hashing, and USB evidence analysis.
- **Activity 2** — MBR / GPT and file-system analysis.
- **Activity 3** — Linux and macOS artifact acquisition and analysis.
- **Activity 4** — Forensic report writing.

### CTF

- **CTF_Apr11** — PESU TraceLabs forensic CTF: challenge notes and solve strategy.

## About the Course

The DFIR laboratory focuses on the practical workflow of a digital investigation — sound evidence acquisition, file-system and memory analysis, artifact recovery across Windows/Linux/macOS, network and browser forensics, and clear forensic reporting.

## Notes on Contents

To keep the repository a reasonable size and within hosting limits, the following are intentionally excluded via `.gitignore`:

- **Forensic evidence**: disk images (`.E01`, `.dd`, `.raw`), memory dumps, browser-cache dumps, registry hives, and large media/video exhibits.
- **Archives**: `.zip`, `.rar`, `.tar`, `.7z` bundles.
- **Third-party tools**: installers and bundled distributions (FTK Imager, Wireshark, RegRipper, Registry Explorer, WinHex, NirSoft viewers, Thunderbird, etc.), all of which are freely downloadable from their vendors.

What remains is the coursework itself: lab reports, analysis documents, exercise files, screenshots, and notes.

**⚠️ Ethical Use Notice:** All material here is for educational use within the course. Forensic techniques must only be applied to evidence and systems you are authorised to examine.

## License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
