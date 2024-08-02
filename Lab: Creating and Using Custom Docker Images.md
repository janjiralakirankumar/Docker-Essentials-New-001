## (Optional Lab) Creating and Using Custom Docker Images

### Objective
This lab demonstrates how to create a custom Docker image from a running container, save changes, and use the new image to create a new container with the modifications.

### Steps

### Step 1: Create a Container Using the Ubuntu Image

Start by pulling the official Ubuntu image and creating a new container.

```bash
docker pull ubuntu
docker run -it --name my_ubuntu_container ubuntu
```

This command starts an interactive shell session inside a new container named `my_ubuntu_container` using the Ubuntu image.

### Step 2: Check for Installed Packages (curl, tree, wget)

Inside the running container, check if the `curl`, `tree`, and `wget` packages are installed.

```bash
which curl
which tree
which wget
```

If the output is empty, the packages are not installed.

### Step 3: Install Packages Manually

Install the missing packages inside the container.

```bash
apt update
apt install -y curl tree wget
```

Verify the installation:

```bash
which curl
which tree
which wget
```

The paths to the executables should now be displayed, confirming the packages are installed.

### Step 4: Commit the Changes and Create an Image

Exit the container:

```bash
exit
```

Commit the changes to create a new Docker image from the modified container. Replace `<your_image_name>` with a name for your new image.

```bash
docker commit my_ubuntu_container <your_image_name>
```

For example:

```bash
docker commit my_ubuntu_container ubuntu_with_tools
```

### Step 5: Check the New Image in the Local Repository

List all the images in your local Docker repository to confirm the new image has been created.

```bash
docker images
```

You should see the new image listed, along with its name and ID.

### Step 6: Create a Container Using the New Image

Create a new container using the newly created image.

```bash
docker run -it --name my_custom_container <your_image_name>
```

For example:

```bash
docker run -it --name my_custom_container ubuntu_with_tools
```

### Step 7: Check the Packages/Changes in the New Container

Inside the new container, verify the installed packages.

```bash
which curl
which tree
which wget
```

The paths to the executables should appear, indicating that the packages are available in the new container.

## Conclusion

In this lab, you learned how to:

1. Start a container using a base image.
2. Install additional packages inside the container.
3. Commit the changes to create a new Docker image.
4. Use the new image to start another container with the modifications.

This process is useful for creating custom Docker images tailored to specific needs, allowing for easy deployment and reproducibility.
