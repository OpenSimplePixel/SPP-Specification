# Simple Pixel Project (SPP) File Specification

## Introduction

### What is Simple Pixel?

**Simple Pixel** is a pixel art editing app for iOS, iPadOS and macOS under development.

### What is the SPP file for?

The **SPP** file is a binary based format for saving project data of Simple Pixel projects. It's designed for high speed reading or writing and low use of storage.

## Basic Informations

- Name: `Simple Pixel Project`
- File extension: `.spp`
- Identifier: `tw.justapple.spp`
- MIME: `application/x-spp`

## File Format

> **Note**: We use **little-endian** for all number values.

| Name | Type | Information |
|---|---|---|
| Magic String | 4 Bytes | Always `SPPF` in ASCII. |
| File Version | UInt8 | Currently `1`. |
| Project Data (Compressed) | LZFSE Compressed Bytes | More details in the section below. |

### Project Data Format

| Name | Type | Information |
|---|---|---|
| Author Name Length | UInt16 | Byte length of author name string. |
| Author Name | UTF-8 Bytes | Length depends on the previous field. |
| Project Description Length | UInt16 | Byte length of project description string. |
| Project Description | UTF-8 Bytes | Length depends on the previous field. |
| Canvas Width | UInt16 | Width of project canvas, of course, in pixel. When this field is `0`, the project will need to be initialized when opened. |
| Canvas Height | UInt16 | Height of project canvas, of course, in pixel too. When this field is `0`, the project will also need to be initialized when opened. |
| User Color Palette Color Count | UInt16 | The number of colors in project's user color palette. |
| User Color Palette Colors | Array of UInt32 | Length depends on the previous field. Each color is encoded in RGBA (4*UInt8). |
| Current Color | UInt32 | The last using color in this project. Encoded in RGBA (4*UInt8). |
| Color Index Color Count | UInt16 | The number of colors in project's color index, generate by system. |
| Color Index Colors | Array of UInt32 | Length depends on the previous field. Each color is encoded in RGBA (4*UInt8). |
| Animation Frame Count | UInt16 | The number of animation frames in this project. |
| Animation Frames | Array of Animation Frame | More details in the section below. |

### Animation Frames Format

| Name | Type | Information |
|---|---|---|
| Duration | Double64 | The duration of this frame in seconds. |
| Layer Count | UInt16 | The number of layers in this frame. |
| Layers | Array of Layer | More details in the section below. |

### Layer

| Name | Type | Information |
|---|---|---|
| Flags | 16 Bits | Bit flag of this frame, (`0b1`: is hidden; `0b10`: use UInt16 index) |
| Pixel Data | Array of UInt8/UInt16 | Length of the array is **Canvas Width** \* **Canvas Height**. Each pixel's color is the id of color index (0~255 or 0~65535). |

