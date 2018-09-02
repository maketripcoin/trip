
Debian
====================
This directory contains files used to package tripd/trip-qt
for Debian-based Linux systems. If you compile tripd/trip-qt yourself, there are some useful files here.

## trip: URI support ##


trip-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install trip-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your tripqt binary to `/usr/bin`
and the `../../share/pixmaps/trip128.png` to `/usr/share/pixmaps`

trip-qt.protocol (KDE)

