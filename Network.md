


# Plan A:

- sudo pacman -S networkmanager
- sudo systemctl enable NetworkManager.service
- sudo systemctl start NetworkManager.service



# Plan B:

- sudo pacman -S wpa_supplicant
- wpa_passphrase networkname passphrase > example.conf
- wpa_supplicant -B -i interface -c example.conf
