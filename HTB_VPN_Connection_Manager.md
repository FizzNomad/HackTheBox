# HTB VPN Connection Manager

This set of Bash functions simplifies the process of connecting and disconnecting from the Hack The Box (HTB) VPN. It's an easy way to select the desired `.ovpn` configuration file when starting a connection, and an easy way to disconnect when you're done.

## Functions

- `htbstart`: Start the VPN connection by selecting an `.ovpn` file
- `htbkill`: Disconnect from the VPN

## Setup

1. Add the `htbstart` and `htbkill` functions to your `.bashrc` or `.bash_aliases` file:

```bash
# HTB VPN Connection Manager

function htbstart() {
  OVPN_CONFIG_DIR=~/.ovpnconfig

  # List all .ovpn files in the directory and let the user select one
  echo "Please select an ovpn file to connect:"
  select ovpn_file in $OVPN_CONFIG_DIR/*.ovpn; do
    if [ -n "$ovpn_file" ]; then
      echo "Starting OpenVPN with $ovpn_file..."
      sudo -v
      sudo openvpn "$ovpn_file" 1>/dev/null 2>&1 &
      echo "Connection to HTB initiated. Have fun storming the castle!"
      break
    else
      echo "Invalid selection. Please try again."
    fi
  done
}

function htbkill() {
  echo "Disconnecting from HTB VPN..."

  # Find and kill the openvpn process
  sudo killall openvpn

  echo "Disconnected from HTB VPN. See you next time!"
}

2. Save the file and either source it (source ~/.bashrc or source ~/.bash_aliases) or restart your terminal for the changes to take effect.

## Usage
1. To start the VPN connection, run `htbstart` in your terminal. You will be prompted to select an `.ovpn` file from your configuration directory 
(`~/.ovpnconfig` by default).

After making a selection, the function will start the OpenVPN connection and display a success message when connected.

When you're ready to disconnect, run `htbkill` in your terminal. The function will find and kill the openvpn process, disconnecting you from the VPN.

Enjoy using the HTB VPN Connection Manager!
