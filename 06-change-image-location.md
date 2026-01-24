# Change Default Image Location

The default image location was C:\Users\toarv\AppData\Local\Docker\wsl

To store your Docker images on the D: drive instead of the default C: drive in Docker Desktop for Windows, you need to move the Docker disk image file to your desired location. Here’s how you can do it:

1. Open Docker Desktop.
2. Go to **Settings**.
3. Select **Resources** > **Advanced**.
4. In the **Disk image location** section, click **Browse** and choose a folder on your D: drive.
5. Click **Apply** to save the changes.

Docker Desktop will move the disk image (which contains all your images and containers) to the new location. Do not move the file manually, as this can cause Docker Desktop to lose track of it.

**Note:** All your images and containers will now be stored in the new location you selected.

Sources:
- [https://docs.docker.com/desktop/troubleshoot-and-support/faqs/linuxfaqs/](https://docs.docker.com/desktop/troubleshoot-and-support/faqs/linuxfaqs/)


I have changed to \
D:\ProgramData\Docker\images\DockerDesktopWSL
