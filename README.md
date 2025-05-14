# myRGB 🌈

Welcome to **myRGB**! This project explores controlling RGB lighting on my new desktop PC running Ubuntu 24.04. Whether you have Corsair, NZXT, or Gigabyte components, this repository aims to provide a seamless way to manage your RGB setup.

![RGB Lighting](https://img.shields.io/badge/RGB%20Control-Available-brightgreen)

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Supported Hardware](#supported-hardware)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Introduction

RGB lighting adds a vibrant touch to any desktop setup. With myRGB, you can control the RGB settings of your hardware using a simple interface. This project focuses on providing an easy-to-use tool for enthusiasts and casual users alike.

For the latest releases, please visit [myRGB Releases](https://github.com/rvillalbag/myRGB/releases).

## Features

- Control RGB lighting for various hardware brands.
- Support for I2C and USB connections.
- Simple command-line interface.
- Compatible with multiple Linux distributions, especially Ubuntu 24.04.
- Easy integration with existing setups.

## Supported Hardware

myRGB supports a variety of hardware brands. Here’s a list of the primary components you can control:

- **Corsair**: RGB RAM, fans, and keyboards.
- **NZXT**: RGB fans and lighting strips.
- **Gigabyte**: RGB motherboards and graphics cards.
- **Kraken**: AIO coolers with RGB support.
- **NVIDIA**: Graphics cards with RGB features.

## Installation

To get started with myRGB, follow these steps:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/rvillalbag/myRGB.git
   cd myRGB
   ```

2. **Install Dependencies**:
   Make sure you have the required packages installed. You can do this using:
   ```bash
   sudo apt install i2c-tools python3 python3-pip
   ```

3. **Download and Execute the Latest Release**:
   For the latest version, download the executable from [myRGB Releases](https://github.com/rvillalbag/myRGB/releases). Follow the instructions provided in the release notes.

## Usage

Once you have installed myRGB, you can start controlling your RGB devices. Here’s how to use it:

1. **Launch the Application**:
   Open your terminal and run:
   ```bash
   ./myRGB
   ```

2. **Basic Commands**:
   - To list connected devices:
     ```bash
     ./myRGB list
     ```
   - To set a specific color:
     ```bash
     ./myRGB set <device_name> <color>
     ```
   - To reset a device:
     ```bash
     ./myRGB reset <device_name>
     ```

3. **Advanced Configuration**:
   For more advanced settings, you can modify the configuration file located in the `config` directory. Adjust parameters as needed to fit your setup.

## Contributing

We welcome contributions! If you want to improve myRGB, follow these steps:

1. **Fork the Repository**: Click the fork button on GitHub.
2. **Create a Branch**: 
   ```bash
   git checkout -b feature-branch
   ```
3. **Make Your Changes**: Edit the code as needed.
4. **Commit Your Changes**: 
   ```bash
   git commit -m "Add new feature"
   ```
5. **Push to Your Fork**: 
   ```bash
   git push origin feature-branch
   ```
6. **Create a Pull Request**: Submit your changes for review.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

If you have any questions or suggestions, feel free to reach out:

- **GitHub**: [rvillalbag](https://github.com/rvillalbag)
- **Email**: [your-email@example.com](mailto:your-email@example.com)

Thank you for checking out **myRGB**! For the latest updates and releases, remember to visit [myRGB Releases](https://github.com/rvillalbag/myRGB/releases). Enjoy controlling your RGB setup!