# 6

# Setting Up Game Servers

As great as it would be to run a Counter Strike server on Raspberry Pi, it is just not possible due to the high requirements of running such game servers. Mostly, this is due to the shortfall of RAM, but some servers also require powerful processors to deliver low-latency performance for all players, which could be up to 24 players per game.

In this chapter, you will be introduced to open source games that have been developed by people with a passion for a particular genre of games. These games were reverse engineered, built from scratch, or just became popular due to their simplicity.

# Updating to Jessie

At the time of writing this book, the main distribution for the Rasbperry Pi was still Debian Wheezy. Debian Jessie was added around June 2013 for testing on the Rasspbery Pi but is already used on desktop machines. As of April 2015, there seems to be a stable version for the Raspberry Pi. A lot of game server packages were updated only in Jessie because of newer dependencies.

You can upgrade the entire Raspberry Pi to Jessie, but you are advised not to do this until the official image is released. Instead, we will install selected, specific packages from Jessie while keeping the entire distribution stable in Wheezy. These steps can be skipped if you are already using Jessie.

# Selective settings

We can add the following two lines to /etc/apt/sources.list:

```
deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi
deb http://archive.raspbian.org/raspbian jessie main contrib non-free rpi
```