# Visual Encryption System: Proof of Concept

## 1. Executive Summary
This document outlines the architecture and workflow for an advanced **Visual Encryption & Steganography System**. This system transforms raw data into visual patterns (pixels) and then intelligently rearranges these pixels to mimic a target image chosen by the user. The result is an encrypted image that visually resembles a recognizable picture, while the original data is recoverable only via a generated **Master Key**.

## 2. System Architecture

The system uses a two-stage process: **Data-to-Color Conversion** and **Pixel Rearrangement (Mimicry)**. A comprehensive **Master Key** is generated to reverse the process.

### Encryption Flow
```mermaid
flowchart TD
    A([Start]) --> B[Input Data]
    B --> C[Convert to Hex String]
    C --> D["Map Hex to Colors\n(Intermediate Noise Image)"]
    D --> E[Select Target Image]
    E --> F["Rearrange Pixels to\nMimic Target Image"]
    F --> G["Generate Master Key\n(Map + Metadata)"]
    F --> H([Output Final Image])
```

### Decryption Flow
```mermaid
flowchart TD
    A([Start]) --> B[Input Final Image]
    B --> C[Load Master Key]
    C --> D["Reverse Pixel Shuffling\n(Using Key)"]
    D --> E["Map Colors to Hex\n(Using Palette)"]
    E --> F[Convert Hex to Data]
    F --> G([Output Original File])
```

## 3. Detailed Components

### 3.1 Input Processing
*   **Input**: Any text string or binary file.
*   **Conversion**: Data is converted to a hexadecimal string representation.

### 3.2 Intermediate Image Generation
*   **Color Mapping**: Hex characters (0-F) are mapped to specific colors using a standard palette.
*   **Result**: An "Intermediate Noise Image" is created, containing the raw encrypted data as colored pixels.

### 3.3 Image Mimicry & Pixel Shuffling
*   **Target Selection**: The user selects a target image (e.g., a landscape, a logo).
*   **Rearrangement Algorithm**: The system analyzes the colors of the "Intermediate Noise Image" and rearranges the pixels to match the visual structure of the "Target Image" as closely as possible.
*   **Output**: A final BMP image that looks like a noisy version of the target image.

### 3.4 The Master Key
A separate file (e.g., `.key` or `.json`) is generated containing all information required for decryption.

**Key Structure:**
*   **File Metadata**:
    *   `Original Filename`: (e.g., `secret.pdf`)
    *   `File Type`: (e.g., `application/pdf`)
*   **Encryption Data**:
    *   `Shuffle Map`: The coordinate mapping to restore pixels to their original order (e.g., `(x_final, y_final) -> (x_original, y_original)`).
    *   `Color Palette`: The specific Hex-to-RGB mapping used.

## 4. Example Scenario

**Input**: `ABC`
1.  **Hex Conversion**: `41 42 43`
2.  **Intermediate Pixels**: `[Yellow, Red, Yellow, Green, Yellow, Blue]`
3.  **Target Image**: A simple flag (Blue, Yellow, Red).
4.  **Shuffling**: The algorithm moves the pixels to match the flag's pattern.
    *   *Final Image*: Looks somewhat like the flag.
5.  **Master Key Generated**: Records that Pixel 1 moved to Pos 5, Pixel 2 to Pos 3, etc., and that the original file was `text.txt`.

## 5. Security Considerations & Limitations
*   **Substitution Cipher**: In its current form, this is a simple substitution cipher. Frequency analysis could potentially break the encryption if the color patterns are analyzed.
*   **Compression**: Lossy compression (like JPEG) will destroy the data. Only lossless formats (BMP, PNG) must be used.
*   **Enhancement**: For true security, the data should be encrypted with AES *before* being converted to hex and mapped to colors.

## 6. Future Improvements
*   Add AES-256 encryption layer before visual mapping.
*   Implement header metadata within the image (e.g., file length) using reserved pixels.
*   Support for shuffling pixel positions based on a seed.
