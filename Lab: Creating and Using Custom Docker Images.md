# (Optional Lab): Creating and Using Custom Docker Images

## Objective
This lab demonstrates how to create a custom Docker image from a running container, save changes, and use the new image to create a new container with the modifications.

## Steps

### Step 1: Create a Container Using the Ubuntu Image

First, pull the official Ubuntu image and create a new container named `ubuntu_temp`:

```bash
docker pull ubuntu
docker run -it --name ubuntu_temp ubuntu
```

Here:
- `docker pull ubuntu`: Downloads the latest Ubuntu image.
- `docker run -it --name ubuntu_temp ubuntu`: Creates and starts a new container named `ubuntu_temp` from the Ubuntu image. The `-it` flag attaches an interactive terminal session to the container.

### Step 2: Check for Installed Packages (`curl`, `tree`, `wget`)

Inside the running container, check if the `curl`, `tree`, and `wget` packages are installed:

```bash
which curl
which tree
which wget
```

The commands will return the paths if the packages are installed. If not, there will be no output.

### Step 3: Install Packages Manually

If the packages are not installed, update the package list and install them:

```bash
apt update
apt install -y curl tree wget
```

- `apt update`: Updates the list of available packages and their versions.
- `apt install -y curl tree wget`: Installs the `curl`, `tree`, and `wget` packages without prompting for confirmation.

Verify the installation:

```bash
which curl
which tree
which wget
```

This time, the paths to the executables should be displayed.

### Step 4: Commit the Changes and Create an Image

Exit the container:

```bash
exit
```

Now, create a new Docker image from the modified container. Name the image `ubuntu_with_tools:v1` (you can choose your naming convention):

```bash
docker commit ubuntu_temp ubuntu_with_tools:v1
```

- `docker commit ubuntu_temp ubuntu_with_tools:v1`: Commits the changes made in the container `ubuntu_temp` and creates a new image named `ubuntu_with_tools` with the tag `v1`.

### Step 5: Check the New Image in the Local Repository

List all the images in your local Docker repository:

```bash
docker images
```

You should see the new image `ubuntu_with_tools:v1` listed.

### Step 6: Create a Container Using the New Image

Now, create a new container named `ubuntu_custom` using the newly created image `ubuntu_with_tools:v1`:

```bash
docker run -it --name ubuntu_custom ubuntu_with_tools:v1
```

- `docker run -it --name ubuntu_custom ubuntu_with_tools:v1`: Creates and starts a new container named `ubuntu_custom` from the `ubuntu_with_tools:v1` image.

### Step 7: Check the Packages/Changes in the New Container

Inside the new container, verify the installed packages:

```bash
which curl
which tree
which wget
```

The commands should return the paths to the installed executables, indicating that the packages are present in the new container.

## Conclusion

In this lab, you:
1. Started a container using the Ubuntu base image.
2. Installed additional packages (`curl`, `tree`, `wget`) inside the container.
3. Committed the changes to create a new Docker image (`ubuntu_with_tools:v1`).
4. Used the new image to start another container (`ubuntu_custom`) with the modifications.

These steps demonstrate how to create and use custom Docker images, which is useful for building environments tailored to specific applications and needs.
