
    compgen -c # shows a list of commands available.
    compgen -c | wc -l # counts the number of commands available on your system

    less # pipe large stdout into this to be able to scroll
    compgen -c | less