pkgname=globexec
pkgver=0.0.2
pkgrel=1
arch=(all)
multiarch=foreign

package(){
  k="${pkgdir}/usr/bin/"
  mkdir -p "$k"
  cd "$k"
  (
    echo '#!/usr/bin/env bash'
    declare -f cpt
    declare -f pass
    declare -f copy
    echo 'copy "${@}"'
  ) > globexec
  chmod 755 globexec
  depends=('bash>=4.2' 'findutils' 'coreutils' 'sed')
}

cpt(){
  if [ -d "$1" ]; then
    (
       cd "$1"
       find . -name '*' -type -f | sed  's/\.\///' | while read line; do
         $4 "$(dirname "$2/$line")"
         $3 "$1/$line" "$2/$line"
       done
    )
  else
    $3 "$1" "$2"
  fi
}

pass(){
  :
}

copy(){
    set -o noglob
    sed -n '/^[[:space:]]*#/!p' | while read line; do
        line1=($line)
        src="${line1[0]}"
        mk="${4:-pass}"
        dest="${line1[1]}"
        line1=($(set +o noglob; cd "${1}"; eval echo "${src}"))
        if [ -z $dest ]; then
            for i in "${line1[@]}"; do
                if [ -e "${1}/${i}" ]; then
                    $mk $(dirname "${2}/${i}")
                    cpt "${1}/${i}" "${2}/${i}" "$3" "$mk"
                fi
            done
        else
            $mk "${2}/${dest}"
            for i in "${line1[@]}"; do
                if [ -e "${1}/${i}" ]; then
                    cpt "${1}/${i}" "${2}/${dest}/$(basename ${i})" "$3" "$mk"
                fi
            done
        fi
    done;
}
