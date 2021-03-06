# kali

## zsh prompt (with IP)

Below will create a prompt with the IP address of the host eth0, or, if on VPN tun0. This can replace the standard `configure_prompt` method in Kali's default `.zshrc` file.

It will look like:

`(<user>ã¿<envrion>)-(<IP>)-[<pwd>]`

```
configure_prompt() {
    prompt_symbol=ã¿

    if ifconfig tun0 >/dev/null 2>&1
    then
        ip='vpn - '$(ifconfig tun0 | grep "inet " | cut -d " " -f 10)
    else
        ip=$(ifconfig eth0 | grep "inet " | cut -d " " -f 10)
    fi

    # Skull emoji for root terminal
    #[ "$EUID" -eq 0 ] && prompt_symbol=ð
    case "$PROMPT_ALTERNATIVE" in
        twoline)
            PROMPT=$'%F{%(#.blue.green)}âââ${debian_chroot:+($debian_chroot)â}${VIRTUAL_ENV:+($(basename $VIRTUAL_ENV))â}(%B%F{%(#.red.blue)}%n'$prompt_symbol$'%m%b%F{%(#.blue.green)})-(%B%F{%(#.red.blue)}'$ip$'%b%F{%(#.blue.green)})-[%B%F{reset}%(6~.%-1~/â¦/%4~.%5~)%b%F{%(#.blue.green)}]\nââ%B%(#.%F{red}#.%F{blue}$)%b%F{reset} '
            # Right-side prompt with exit codes and background processes
            #RPROMPT=$'%(?.. %? %F{red}%Bâ¨¯%b%F{reset})%(1j. %j %F{yellow}%Bâ%b%F{reset}.)'
            ;;
        oneline)
            PROMPT=$'${debian_chroot:+($debian_chroot)}${VIRTUAL_ENV:+($(basename $VIRTUAL_ENV))}%B%F{%(#.red.blue)}%n@%m%b%F{reset}:%B%F{%(#.blue.green)}%~%b%F{reset}%(#.#.$) '
            RPROMPT=
            ;;
        backtrack)
            PROMPT=$'${debian_chroot:+($debian_chroot)}${VIRTUAL_ENV:+($(basename $VIRTUAL_ENV))}%B%F{red}%n@%m%b%F{reset}:%B%F{blue}%~%b%F{reset}%(#.#.$) '
            RPROMPT=
            ;;
    esac
    unset prompt_symbol
}
```