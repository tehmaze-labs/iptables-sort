# iptables-sort

Like iptables-save, but sorted.

## BIG FAT WARNING

> This software is nowhere near production ready, do *not* use on production
> systems. I repeat: do *not* use. You have been warned hm'kay.

## About

In the iptables firewall, rules are evaluated from first to last rule. On
ACCEPT or DROP rules, if a rule is matched, it will stop evaluating rules.
This means that the firewall will have to keep matching packets against the
configured rules until it finds a hit. Processing these rules are mostly
CPU bound and therefor can turn out to be a bottle neck on loaded systems.

iptables-sort attempts to limit spending CPU cycles on rules that are
inefficiently ordered by organising the iptables rules based on packet counts.
Rules that belong to the same chain and target will be grouped.

## Usage

The most basic usage is:

    # iptables-sort | iptables-restore


For all available options, refer to the inline help:

    # iptables-sort --help
    Usage: iptables-sort [options]

    Options:
      -h, --help            show this help message and exit
      -c GLOB, --ignore-chain=GLOB
                            Skip re-ordering rules in chain
      -t GLOB, --ignore-target=GLOB
                            Skip re ordering rules in target
      -B, --sort-bytes      Sort by bytes rather than packet counts (default: no)


So, to prevent rules from being re-ordered in the default chains:

    # iptables-sort \
        -c INPUT,OUTPUT,FORWARD \
        -c \*ROUTING \
        | iptables-restore

The `*ROUTING` glob will match both `PREROUSTING` and `POSTROUTING` chains.

## Bugs

Don't make me say I told you so ;-)

You can file issues in the
[GitHub bug tracker](https://github.com/tehmaze/iptables-sort/issues).
