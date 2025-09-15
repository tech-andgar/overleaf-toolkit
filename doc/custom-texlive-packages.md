# How to Install Custom TeX Live Packages

This guide explains how to install additional TeX Live packages and make them permanent.

## Automated Scripts

### Install a Single Package

A script is available to automate the process of adding a new TeX Live package.

To use the script, run the following command, replacing `<package-name>` with the name of the package you want to install:

```bash
./bin/add-tex-package <package-name>
```

For example:
```bash
./bin/add-tex-package fontawesome5
```

The script will automatically:
1.  Install the package.
2.  Commit the changes to a new Docker image.
3.  Update the `config/version` file.
4.  Restart the Overleaf application.

### Install a Set of Essential Packages

If you frequently need the same set of packages, you can use the `install-essential-packages` script. This script installs a predefined list of packages (`titlesec`, `enumitem`, `fontawesome5`). It first checks if each package is already installed and accessible to LaTeX using `kpsewhich` before attempting installation.

To use the script, run the following command:
```bash
./bin/install-essential-packages
```

You can customize the list of packages by editing the `ESSENTIAL_PACKAGES` array at the beginning of the `bin/install-essential-packages` script.

---

## Manual Installation

If you prefer to perform the steps manually, follow the guide below.

The Overleaf Docker image includes a minimal TeX Live installation to save space. If your LaTeX project requires packages that are not included in the base installation, you will encounter a "File not found" error.

This guide explains how to install additional TeX Live packages and make them permanent.

## Step 1: Install TeX Live Packages

You can either install packages individually or install the complete TeX Live distribution.

### Option A: Install a Single Package

If you only need one or two packages, you can install them individually.

1.  Open a shell inside the `sharelatex` container:
    ```bash
    ./bin/shell
    ```
    Alternatively, you can run commands directly using `docker exec`:
    ```bash
    docker exec -it sharelatex /bin/bash
    ```

2.  Inside the container, use the `tlmgr` command to install the desired package. For example, to install `titlesec`:
    ```bash
    tlmgr install titlesec
    ```

3.  After installing the package, you need to update the TeX Live database:
    ```bash
    tlmgr path add
    ```

### Option B: Install the Full TeX Live Distribution

If you need many packages, it may be easier to install the complete TeX Live distribution (`scheme-full`). This is a large download and will take a significant amount of time and disk space.

1.  Run the following command to install `scheme-full`:
    ```bash
    docker exec sharelatex tlmgr install scheme-full
    ```

2.  After the installation is complete, update the TeX Live database:
    ```bash
    docker exec sharelatex tlmgr path add
    ```

## Step 2: Make the Changes Persistent

The changes you've made to the container are temporary and will be lost if the container is recreated. To make them permanent, you need to commit the changes to a new Docker image.

1.  Commit the `sharelatex` container to a new image. Choose a descriptive tag for your new image. For example, if your current version is `5.5.4` and you installed the `titlesec` package, you could use `5.5.4-with-titlesec`.
    ```bash
    docker commit sharelatex sharelatex/sharelatex:5.5.4-with-titlesec
    ```
    If you installed the full distribution, you could use a tag like `5.5.4-with-texlive-full`.

2.  Update the `config/version` file with your new image tag:
    ```bash
    echo "5.5.4-with-titlesec" > config/version
    ```

## Step 3: Fix the Version Validation Script (If Necessary)

The `bin/up` script has a strict version validation rule that may not allow for custom image tags. If you encounter an "invalid version" error when running `bin/up`, you will need to modify the validation script.

1.  Open the file `lib/shared-functions.sh`.

2.  Find the `read_image_version` function.

3.  Modify the regular expression to allow for more general custom tags.

    **Original line:**
    ```bash
    if [[ ! "$IMAGE_VERSION" =~ ^([0-9]+)\\.([0-9]+)\\.([0-9])+(-RC[0-9]*)?(-with-texlive-full)?$ ]]
    ```

    **Modified line:**
    ```bash
    if [[ ! "$IMAGE_VERSION" =~ ^([0-9]+)\\.([0-9]+)\\.([0-9])+(-[a-zA-Z0-9-]+)?$ ]]
    ```

## Step 4: Apply the Changes

Finally, run `bin/up` to restart the Overleaf stack with your new image:
```bash
./bin/up
```

Your new TeX Live packages should now be available in Overleaf.
