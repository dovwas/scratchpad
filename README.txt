This is my first public GitHub file. Quite historic, no?

____________________________________________________________________________________________________
Generic .prompt:
____________________________________________________________________________________________________
# This function returns "/path/to/dir" for non perforce directories,
# or client_name://google3/maps/frontend for p4 dirs.
function get_nice_pwd {
    prefix=/home/dovwas/g4clients/
    input="$(pwd)"
    unset client
    unset google3dir

    if [[ $input =~ $prefix([a-z]+) ]]; then
        client=${BASH_REMATCH[1]}
    fi
    if [[ -z $client ]]; then
        echo $(pwd)
        return
    fi

    basedir="$prefix$client/"
    if [[ $input =~ $basedir(.+) ]]; then
        google3dir=${BASH_REMATCH[1]}
    fi

    echo $client://$google3dir
}

function make_color_code {
    if [ \! -z "$1" ]; then
        color_code=$1
        shift
        echo -n "$(tput setaf $color_code)${*}$(tput sgr0)"
    fi
}

function start_color_code {
    if [ \! -z "$1" ]; then
        color_code=$1
        shift
        echo -n "$(tput setaf $color_code)"
    fi
}

function reset_color {
    echo -n "$(tput sgr0)"
}

function make_red {
    make_color_code 1 $*
}

function make_green {
    make_color_code 2 $*
}

function make_blue {
    make_color_code 4 $*
}

function make_bold {
    if [ \! -z "$1" ]; then
        echo -n "$(tput bold)${*}$(tput sgr0)"
    fi
}

function show_error {
    make_bold $(make_red $*)
}

function get_nice_date {
    make_blue `date '+%a %b %d %R:%S'`
}

function make_nice_rc {
    if [ \! -z "$1" ]; then
        local retcode=$1
        if [ $retcode -ne 0 ]; then
            make_red "[rc=$retcode]"
            echo -n " "
        fi
    else
        make_red "[retcode unknown]"
        echo -n " "
    fi
}

export PS1="\$(make_nice_rc \$?)\$(get_nice_date) \! $(start_color_code 3)\u@\\h$(reset_color) \$(get_nice_pwd) $(make_bold $(make_blue '»')) "

# eof
____________________________________________________________________________________________________

