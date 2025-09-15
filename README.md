# Overleaf Toolkit

This repository contains the Overleaf Toolkit, the standard tools for running a local
instance of [Overleaf](https://overleaf.com). This toolkit will help you to set up and administer both Overleaf Community Edition (free to use, and community supported), and Overleaf Server Pro (commercial, with professional support).

The [Developer wiki](https://github.com/overleaf/overleaf/wiki) contains further documentation on releases, features and other configuration elements.


## Getting Started

Clone this repository locally:

``` sh
git clone https://github.com/overleaf/toolkit.git ./overleaf-toolkit
```

Then follow the [Quick Start Guide](./doc/quick-start-guide.md).

## TeX Live Package Management for Developers

When using minimal Overleaf Docker images, you might encounter "LaTeX Error: File '<package>.sty' not found." This indicates a missing TeX Live package. This toolkit automatically detects your CPU architecture (ARM or x86_64) and uses an appropriate Docker image.

We provide scripts to simplify package management:

-   **Install Essential Packages:** Use `./bin/install-essential-packages` to install a predefined set of common LaTeX packages. This script intelligently checks if packages are already installed and accessible to LaTeX before attempting installation.
-   **Install Individual Packages:** Use `./bin/add-tex-package <package-name>` to install a specific LaTeX package.

For a complete guide on TeX Live package management, including manual installation steps and troubleshooting, refer to [Custom TeX Live Packages](./doc/custom-texlive-packages.md).

To reset your project to a clean state (removing containers, images, data, and config files), use `./bin/reset-project`.


## Documentation

See [Documentation Index](./doc/README.md)


## Contributing

See the [CONTRIBUTING](https://github.com/overleaf/overleaf/blob/main/CONTRIBUTING.md) file.


## Getting Help

Users of the free Community Edition should [open an issue on github](https://github.com/overleaf/toolkit/issues). 

Users of Server Pro should contact `support@overleaf.com` for assistance.

In both cases, it is a good idea to include the output of the `bin/doctor` script in your message.

