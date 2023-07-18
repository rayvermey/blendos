Hi, here is my procedure i use for building a BlendOS iso from the host itself
Building an iso in a container does not work so i have to use the BlendOs host itself.
I dont want to have to install all kind of needed packages because that goes directly against
the BlendOs principles.
So with the help of @rs2009 I made it work as follows:

```
mkdir arch-chroot
```
```
sudo pacstrap -K archroot base linux linux-firmware base-devel vim sudo
```
```
sudo mount -o rw,suid --bind arch-chroot arch-chroot
```
```
sudo arch-chroot arch-chroot
```
```
useradd -m YOUR USERNAME
```
add YOUR USERNAME to sudoers
```
su - YOUR USERNAME
```

YOUR USERNAME is an user you create cause building packages under root is not possible

```
mkdir blend-iso
```
```
sudo pacman -Syu
```
```
sudo pacman -S python-click python-yaml python-psutil grub squashfs-tools archiso git
```

```vi get_assemble.sh```    #and copy paste lines between the ------ below in there

---------------------------------
```
TEMP_ASSEMBLE_DIR="$(mktemp -d)"
git clone https://github.com/blend-os/assemble "${TEMP_ASSEMBLE_DIR}/assemble"
sudo cp "${TEMP_ASSEMBLE_DIR}/assemble/assemble" /usr/local/bin
rm -rf "${TEMP_ASSEMBLE_DIR}"
```
--------------------------------

```
sh get_assemble.sh
```

```
cd blend-iso
```
```
assemble init https://github.com/blend-os/manifests main
```
```
assemble sync
```

To make sure all packages that breakfast downloads are indeed downloaded
do this:

```
sudo vim /etc/pacman.conf
```

add SigLevel = Never     under all active repositories, this makes sure there will be no gpg key errors which sometimes happen, don't know why that is.
```
sudo pacman -Syy
```

```
source build/envsetup.sh
```

```
breakfast | tee breakfast.log
```

If you see at the end all packages are built succesfully you can start:

```
sudo brunch
```

Pick the iso you want to create from the list.

If the iso is built succesfully you will see a line where the new created iso resides.
