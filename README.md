Windows Netboot
===============

x64 is required by iPXE. It's actually just the `media` directory from a WinPE build.

Unpack Image
------------

### WinPE
01. Mount the WinPE image (must be to disk not share)
    ```
    dism /mount-image /imagefile:"F:\windows\x64\sources\boot.wim" /index:1 /mountdir:"C:\WinPE_amd64\mount"
    ```

02. Perform whatever changes you need to the image. Come back here when you're done.

03. Unmount the extracted image.
    ```
    dism /Unmount-Image /Mountdir:"C:\WinPE_amd64\mount" /commit
    ```

### Installer

01. Identify the image index to extract.
    ```
    dism /Get-ImageInfo  /imagefile:"F:\windows\Installer\sources\install.esd"
    ```

02. Extract that image from `install.esd` to a random WIM file.
    ```
    dism /export-image /SourceImageFile:"F:\windows\Installer\sources\install.esd" `
    /SourceIndex:7 /destinationimagefile:"F:\windows\Installer\sources\win10pro.wim" `
    /Compress:max /CheckIntegrity
    ```

03. Mount the extracted image.
    ```
    dism /Mount-Image /ImageFile:"F:\windows\Installer\sources\win10pro.wim" /MountDir:"C:\WinPE_amd64\mount" /index:1
    ```

04. Perform whatever changes you need to the image. Come back here when you're done.

05. Unmount the extracted image.
    ```
    dism /Unmount-Image /Mountdir:"C:\WinPE_amd64\mount" /commit
    ```

06. "Export" the modified image back to the original install ESD.
    ```
    dism /export-image /sourceimagefile:"F:\windows\Installer\sources\win10pro.wim" `
    /sourceindex:1 /destinationimagefile:"F:\windows\Installer\sources\install.esd" `
    /compress:recovery
    ```

07. Delete the index of the original image you extracted.
    ```
    dism /delete-image /imagefile:"F:\windows\Installer\sources\install.esd" /index:7
    ```

Drivers
-------
```
dism /add-driver /image:"C:\WinPE_amd64\mount" /driver:"F:\Drivers\NetKVM\w10\amd64\netkvm.inf"
```
