# Apparmor profiles for Monero on Tails

Experimental apparmor profiles for Tails.

With some minor modifications you should be able to get it to work on other Linux systems as well. Replace 'amnesia' with the name of your home directory and see if it works. The profiles assume you proxy the Monero traffic over Tor via torsocks.

## Download and Installation

Download the apparmor profiles:

        git clone https://github.com/garlicgambit/apparmor-monero-tails
        cd apparmor-monero-tails

Load the apparmor profiles one by one:

        sudo apparmor_parser -r monerod
        sudo apparmor_parser -r monero_wallet_cli
        sudo apparmor_parser -r monero_wallet_gui

Or load them all at once:

        sudo apparmor_parser -r monero*

**Important:** Every time you boot Tails you need to load the profiles. It might be a good idea to store them in a persistent directory so you don't need to download them after each reboot.

## Location of Monero data and binaries

The profiles assume that the Monero data and binaries are located in one of the following directories:

        /home/amnesia/monero.../
        /home/amnesia/Desktop/monero.../
        /home/amnesia/Persistent/monero.../
        /media/amnesia/TailsData/Persistent/monero.../
        /media/amnesia/*/monero.../
        /media/amnesia/*/Persistent/monero.../
        /lib/live/mount/medium/monero.../

... = you can add other characters after 'monero'.

You can also replace 'monero' with '.bitmonero', 'bitmonero' and 'Monero...'.

Directory examples:

        /home/amnesia/monero-gui-linux-x64-v0.10.3.1/
        /home/amnesia/Persistent/.bitmonero/
        /home/amnesia/Persistent/Monero_wallets/

## Verify that apparmor is confining the Monero binaries

Run `aa-status` and check that 'monero\_wallet\_cli', 'monero\_wallet\_gui' and 'monerod' are listed under 'X profiles are in enforce mode':

        sudo aa-status

Then run a Monero binary. Example:

        DNS_PUBLIC=tcp torsocks ./monerod --p2p-bind-ip 127.0.0.1 --no-igd --rpc-bind-ip 127.0.0.1 --data-dir /home/amnesia/Persistent/monero/
        torsocks ./monero-wallet-cli
        torsocks ./monero-wallet-gui
        torsocks ./start-gui.sh

And run `aa-status` again and check that the binary is listed under 'X processes are in enforce mode'

        sudo aa-status

## Troubleshooting and Maintenance

Check if apparmor is blocking an action:

        sudo dmesg |grep DENIED

Update one or all Monero apparmor profiles:

        sudo apparmor_parser -r monerod
        sudo apparmor_parser -r monero*

Disable one or all Monero apparmor profiles:

        sudo apparmor_parser -R monerod
        sudo apparmor_parser -R monero*

## License

MIT

## Donate and Support

Monero: `463DQj1ebHSWrsyuFTfHSTDaACx3WZtmMFMwb6QEX7asGyUBaRe2fHbhMchpZnaQ6XKXcHZLq8Vt1BRSLpbqdr283QinCRK`

Bitcoin: `183x37Wc3jfduKGa5umqHt2gW7tgqWcbWh`

Website: [garlicgambit.wordpress.com](https://garlicgambit.wordpress.com)
